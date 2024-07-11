# Shuffle songs 

## Fisher-Yates 

The Fisher-Yates shuffle algorithm is used in Spotify to randomize the order of songs during shuffle play. It iteratively selects a random song from the current list and swaps it with the song at the current position, 
ensuring every song has an equal chance of appearing next. This process creates a fair and unpredictable sequence, enhancing user experience by introducing variety in playlist playback.


![alt text](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhpimtwdKgMV_QTI0UU12m3NIO0FUZsryqiK4MuYfBO6hl4ED-jXy2Ya3Goc5cS1GeggIvNWDM6MzJQexqSBqFOu4HC8d0dRTxyPhYnxhscQd640YPErFMLjVk9GVscaH4dZzhRYUB3FCI/s1600/FisherYatesAnim.gif)

## Code 

```cpp
#include <iostream>
#include <vector>
#include <cstdlib> 
#include <ctime>   

using namespace std;

void shuffleSongs(vector<string> &songs) {
    srand(time(0));

    int n = songs.size();
    for (int i = n - 1; i > 0; --i) {
        int j = rand() % (i + 1);
        string temp = songs[i];
        songs[i] = songs[j];
        songs[j] = temp;
    }
}

int main() {
   
    vector<string> songs = {"Song 1", "Song 2", "Song 3", "Song 4", "Song 5"};

    cout << "Original order:" << endl;
    for (const string &song : songs) {
        cout << song << " ";
    }
    cout << endl;

 
    shuffleSongs(songs);

    cout << "Shuffled order:" << endl;
    for (const string &song : songs) {
        cout << song << " ";
    }
    cout << endl;

    return 0;
}
```
