# MusicBrainz_Artist_Search
This program allows users to search for music artists using the MusicBrainz API. It takes the artist's name as input, sends an HTTP request, and prints out the names of matching artists in the response.
# Features
* Searches for artists by name using the MusicBrainz API.

* Parses and displays JSON data in a user-friendly format.

* Handles cases when no results are found.

* Includes basic error handling for HTTP requests.

# How to Use
* Run the Program
 Launch the program using your Python interpreter or via Replit.

* Enter an Artist Name
 Type in the name of the artist you want to search for.

* View Results
The program will display a list of matching artist names retrieved from the MusicBrainz database.

# Technical Details
* Language: Python 3

* Libraries: requests

* API Used: MusicBrainz Web Service

* HTTP Method: GET

* Response Format: JSON

  # Code Snippet

  ```
import requests
from requests.exceptions import RequestException
from json import JSONDecodeError

def search_artist(artist_name):
    # Ensure the input is not just empty whitespace
    if not artist_name.strip():
        print("Error: Artist name cannot be empty.")
        return

    url = f"http://musicbrainz.org/ws/2/artist/?query={artist_name}&fmt=json"

    try:
        # Add timeout to avoid hanging, and set required User-Agent
        response = requests.get(
            url,
            headers={'User-Agent': 'MusicSearchApp/1.0 (example@example.com)'},
            timeout=10
        )
        response.raise_for_status()

        try:
            data = response.json()
        except JSONDecodeError:
            print("Error: Unable to parse response (invalid JSON).")
            return

        # Safely check for 'artists' key
        if 'artists' in data and data['artists']:
            print(f"Found {len(data['artists'])} result(s) for '{artist_name}':")
            for artist in data['artists']:
                print(f"- {artist['name']}")
        else:
            print(f"No results found for '{artist_name}'.")

    except requests.exceptions.Timeout:
        print("Error: The request timed out. Please try again later.")
    except requests.exceptions.HTTPError as http_err:
        if response.status_code in [429, 503]:
            print("Error: Rate limit exceeded or service unavailable. Please wait and try again.")
        else:
            print(f"HTTP Error: {http_err}")
    except RequestException as e:
        print(f"An error occurred while fetching data: {e}")

def main():
    artist_name = input("Enter artist name: ")
    search_artist(artist_name)

if __name__ == "__main__":
    main()
    
```
