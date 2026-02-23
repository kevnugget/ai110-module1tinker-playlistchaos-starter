# Playlist Chaos

This application aims to build a smart playlist generator, but there are numerous errors within the application and code. We will use AI to help us debug and fix common errors within it.

---

## Errors Found

The interface has the following errors that I was able to find:

    - The "Hype Ratio" being incorrect at 1.00 and not calculating the ratio of hype songs to total songs
    - The search function is not working correctly as typing a name or a part of a name does not give us the expected result
    - When songs are given attributes of both chill and hype, such an energy level of 7 and a lofi genre, it will go the Hype playlist. It should go to the Mixed playlist since it is not clearly defined to fit into a single playlist
    - The history is not currently tracking anything and is empty

## Approaching Each Error

For each error, we want to look at the code and see what is wrong with it.
    - For the Hype Ratio, we notice that it is correctly dividing the total number of hype songs, by the length of the hype playlist, always giving us a ratio of 1.00
        - Here, we want to divide total, which is len(hype), by the length of all the songs in the playlist (len(all_songs))

