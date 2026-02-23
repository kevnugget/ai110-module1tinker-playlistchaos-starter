# Playlist Chaos

This application aims to build a smart playlist generator, but there are numerous errors within the application and code. We will use AI to help us debug and fix common errors within it.

---

## Errors Found

The interface has the following errors that I was able to find:

- The "Hype Ratio" being incorrect at 1.00 and not calculating the ratio of hype songs to total songs
- The search function is not working correctly as typing a name or a part of a name does not give us the expected result
- When songs are given attributes of both chill and hype, such an energy level of 7 and a lofi genre, it will go the Hype playlist. It should go to the Mixed playlist since it is not clearly defined to fit into a single playlist
- The 'Feeling Lucky' button is not playing Mixed songs, and is only playing Chill or Hype songs. 
- History is not tracking mixed songs
- The average energy is incorrectly tracked as it only summed the songs in the hype playlists instead of all songs

## Approaching Each Error

For each error, we want to look at the code and see what is wrong with it.

- For the Hype Ratio, we notice that it is correctly dividing the total number of hype songs, by the length of the hype playlist, always giving us a ratio of 1.00
    - Here, we want to divide total, which is len(hype), by the length of all the songs in the playlist (len(all_songs))
    ```python
    total = len(hype)
    hype_ratio = total / len(all_songs) if total > 0 else 0.0
    ```

- For the search function, whenever we would search an artist, we would get an empty list
    - When we look at the search_songs function, we saw that the function originally checked if the song's artist is found in the query. However, since we want to check if the query is within the song's artist, we want to swap the check.
        - We want to check if the query is in the song's artist, so instead of
        ```python
        if value and value in q:
        ```
        which was the original code, we do
        ```python
        if value and q in value
        ```
        to ensure that we are searching with our query.

- When we are classifying songs, if they share similar traits as both Chill and Hype songs, it should go into the Mixed playlists. The current classify_song function only checks if it belongs to only one classification, without checking if it also qualifies for the other attribute.
    - We want to check if a song has both chill and hype characteristics. If it does, add it to the Mixed playlists.
    ```python
    is_hype = genre == favorite_genre or energy >= hype_min_energy or is_hype_keyword
    is_chill = energy <= chill_max_energy, is_chill_keyword

    if is_hype and is_chill:
        return "Mixed"
    if is_hype:
        return "Hype"
    if is_chill:
        return "Chill"
    return "Mixed"
    ```
    - This allows us to verify if a song has characteristics to both hype and chill songs. If it does, we want to add it to the Mixed playlist. If it doesn't, add it to its respective playlist.
    
- For the "Feeling Lucky" song selection, we want to include "Mixed" songs instead of only playing "Chill" and "Hype" songs
    - We went to the lucky_section function in app.py and updated the options list to allow the user to generate songs from the Mixed playlist
    ```python
    options=["any", "hype", "chill", "mixed"]
    ```
    - We also went to the lucky_pick function in playlist_logic.py to randomly pick a song from the "Mixed" playlist
        - We did this by adding a conditional to include the Mixed playlists if the user chose the "Mixed playlist" or "Any" to include the Mixed playlist into the list of all songs
        ```python
        elif mode == 'mixed':
            songs = playlists.get("Mixed", [])
        else:
            songs = playlists.get("Hype", []) + playlists.get("Chill", []) + playlists.get("Mixed", [])
        ``` 
    - This also fixes the History error as it now accurately tracks the Chill, Hype, and Mixed songs randomly picked for the user.

- For the average energy, we saw that it only summed the energy of songs in the hype playlists.
    - We want to calculate the average energy through all playlists, so we want to iterate through the songs in the all_songs list.
        - We replaced the iterator 
        ```python
        for song in hype
        ```
        with
        ```python
        for song in all_songs
        ``` 
        to accurately track ALL energies.