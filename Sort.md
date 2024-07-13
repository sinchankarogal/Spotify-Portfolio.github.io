# Sorting of songs in a playlist 

## Quick sort 

Quick Sort is an efficient sorting algorithm that can be used to sort songs based on different attributes like duration, artist, and title.

![alt text](https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.quora.com%2FWhats-your-own-method-for-organizing-songs-in-Spotify&psig=AOvVaw2wcorsgMUd6Z_wJ9d02SR_&ust=1720959796880000&source=images&cd=vfe&opi=89978449&ved=0CBEQjRxqFwoTCKi8nvyApIcDFQAAAAAdAAAAABAR)

### Code

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

struct Song {
    string title;
    string artist;
    int duration; // Duration in seconds
};

int partition(vector<Song>& songs, int low, int high, char criterion) {
    Song pivot = songs[high];
    int i = low - 1;

    for (int j = low; j <= high - 1; ++j) {
        bool condition = false;
        if (criterion == 'd') condition = (songs[j].duration < pivot.duration);
        else if (criterion == 'a') condition = (songs[j].artist < pivot.artist);
        else if (criterion == 't') condition = (songs[j].title < pivot.title);

        if (condition) {
            i++;
            swap(songs[i], songs[j]);
        }
    }
    swap(songs[i + 1], songs[high]);
    return (i + 1);
}

void quickSort(vector<Song>& songs, int low, int high, char criterion) {
    if (low < high) {
        int pi = partition(songs, low, high, criterion);
        quickSort(songs, low, pi - 1, criterion);
        quickSort(songs, pi + 1, high, criterion);
    }
}

int main() {
    vector<Song> songs = {
        {"Shape of You", "Ed Sheeran", 240},
        {"Blinding Lights", "The Weeknd", 200},
        {"Dance Monkey", "Tones and I", 210},
        {"Rockstar", "Post Malone", 230},
        {"Circles", "Post Malone", 215},
        {"Someone Like You", "Adele", 285},
        {"Despacito", "Luis Fonsi", 229}
    };

    char criterion;
    cout << "Sort by (d)uration, (a)rtist, or (t)itle: ";
    cin >> criterion;

    quickSort(songs, 0, songs.size() - 1, criterion);

    cout << "Sorted songs:\n";
    for (const auto& song : songs) {
        cout << song.title << " by " << song.artist << " (" << song.duration << " seconds)\n";
    }

    return 0;
}
```
