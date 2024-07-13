# Sorting of songs in a playlist 

## Quick sort 

Quick Sort is an efficient sorting algorithm that can be used to sort songs based on different attributes like duration, artist, and title.

![alt text](https://www.cnet.com/a/img/resize/42897897174ace5b7d88bf2ab8a7de2eae276a03/hub/2022/03/11/253590a9-7131-4f48-b10c-d0896d0d7d85/img-3816.jpg?auto=webp&width=768)

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
