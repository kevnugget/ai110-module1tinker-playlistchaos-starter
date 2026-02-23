# Playlist Chaos

This application aims to build a smart playlist generator, but there are numerous errors within the application and code. We will use AI to help us debug and fix common errors within it.

---

## Errors Found

The interface has the following errors that I was able to find:

- The "Hype Ratio" being incorrect at 1.00 and not calculating the ratio of hype songs to total songs
- The search function is not working correctly as typing a name or a part of a name does not give us the expected result
- When songs are given attributes of both chill and hype, such an energy level of 7 and a lofi genre, it will go the Hype playlist. It should go to the Mixed playlist since it is not clearly defined to fit into a single playlist
- The history is not currently tracking anything and is empty
- The average energy is incorrectly tracked as it only summed the songs in the hype playlists instead of all songs

## Approaching Each Error

For each error, we want to look at the code and see what is wrong with it.

- For the Hype Ratio, we notice that it is correctly dividing the total number of hype songs, by the length of the hype playlist, always giving us a ratio of 1.00
    - Here, we want to divide total, which is len(hype), by the length of all the songs in the playlist (len(all_songs))

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

