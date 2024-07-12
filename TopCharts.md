# Top Charts

## Max-Heap

![alt text](https://media.geeksforgeeks.org/wp-content/uploads/20230901130152/Insertion-In-Max-Heap.png)

Using a max-heap for maintaining top charts ensures efficient updates and retrievals. When a song's play count or rating is updated, it is inserted into the max-heap in \(O(log n)\) time,
keeping the heap balanced. Retrieving the top \(k\) songs is efficient, requiring \(k\) extractions from the heap, each taking \(O(log n)\) time,
which ensures that the highest play counts are always at the top. This makes it ideal for dynamically maintaining and accessing top charts in Spotify.


### code 

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <unordered_map>
using namespace std;

class Song {
public:
    string title;
    int playCount;
    
    Song(string t, int p) : title(t), playCount(p) {}
    
    // Comparator for max-heap based on playCount
    bool operator<(const Song& other) const {
        return playCount < other.playCount;
    }
};

class TopCharts {
private:
    priority_queue<Song> maxHeap;
    unordered_map<string, int> songPlayCounts;

public:
    void addOrUpdateSong(string title, int playCount) {
        songPlayCounts[title] = playCount;
        maxHeap.push(Song(title, playCount));
    }

    vector<string> getTopKSongs(int k) {
        vector<string> topKSongs;
        priority_queue<Song> tempHeap = maxHeap;

        for (int i = 0; i < k && !tempHeap.empty(); ++i) {
            topKSongs.push_back(tempHeap.top().title);
            tempHeap.pop();
        }
        return topKSongs;
    }
};

int main() {
    TopCharts charts;
    charts.addOrUpdateSong("Shape of You", 100);
    charts.addOrUpdateSong("Blinding Lights", 150);
    charts.addOrUpdateSong("Dance Monkey", 120);
    charts.addOrUpdateSong("Rockstar", 130);

    int k = 3;
    vector<string> topSongs = charts.getTopKSongs(k);

    cout << "Top " << k << " songs:" << endl;
    for (const string& song : topSongs) {
        cout << song << endl;
    }
    return 0;
}

```
