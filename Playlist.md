# Playlist Management 

## Doubly Linked List 

A doubly linked list can manage Spotify playlists by maintaining the order of songs, allowing efficient insertion and deletion operations, 
and providing easy navigation through the playlist in both forward and backward directions.


![alt text](https://www.equestionanswers.com/c/images/doubly-linked-list.gif)

### code 

```cpp
#include <iostream>
#include <string>

using namespace std;

// Node structure for the doubly linked list
struct SongNode {
    string title;
    int id;
    SongNode* prev;
    SongNode* next;
    SongNode(string t, int i) : title(t), id(i), prev(nullptr), next(nullptr) {}
};

// Playlist class managing the doubly linked list
class PlaylistDLL {
public:
    SongNode* head;
    SongNode* tail;

    PlaylistDLL() : head(nullptr), tail(nullptr) {}

    // Add a song to the end of the playlist
    void addSong(string title, int id) {
        SongNode* newSong = new SongNode(title, id);
        if (!head) {
            head = tail = newSong;
        } else {
            tail->next = newSong;
            newSong->prev = tail;
            tail = newSong;
        }
    }

    // Remove a song by ID
    void removeSong(int id) {
        SongNode* current = head;
        while (current) {
            if (current->id == id) {
                if (current->prev) current->prev->next = current->next;
                if (current->next) current->next->prev = current->prev;
                if (current == head) head = current->next;
                if (current == tail) tail = current->prev;
                delete current;
                return;
            }
            current = current->next;
        }
    }

    // Display the playlist
    void displayPlaylist() {
        SongNode* current = head;
        while (current) {
            cout << "ID: " << current->id << ", Title: " << current->title << endl;
            current = current->next;
        }
    }
};

int main() {
    PlaylistDLL playlist;

    // Add songs to the playlist
    playlist.addSong("Shape of You", 1);
    playlist.addSong("Blinding Lights", 2);
    playlist.addSong("Dance Monkey", 3);

    // Display the playlist
    cout << "Playlist:" << endl;
    playlist.displayPlaylist();

    // Remove a song
    playlist.removeSong(1);
    cout << "Playlist after removing song with ID 1:" << endl;
    playlist.displayPlaylist();

    return 0;
}

```


## Hash Map

A hash map can manage Spotify playlists by enabling fast lookups, additions, and deletions of songs using song IDs as keys, making it
efficient to quickly access and modify songs in the playlist.

![alt text](https://miro.medium.com/v2/resize:fit:1400/1*8hAoOurJFBfA-w1wKUAqIA.gif)

### code 

```cpp
#include <iostream>
#include <unordered_map>
#include <string>

using namespace std;

// Playlist class managing the hash map
class PlaylistHashMap {
public:
    unordered_map<int, string> songMap;

    // Add a song to the playlist
    void addSong(string title, int id) {
        songMap[id] = title;
    }

    // Remove a song by ID
    void removeSong(int id) {
        songMap.erase(id);
    }

    // Display the playlist
    void displayPlaylist() {
        for (const auto& pair : songMap) {
            cout << "ID: " << pair.first << ", Title: " << pair.second << endl;
        }
    }
};

int main() {
    PlaylistHashMap playlist;

    // Add songs to the playlist
    playlist.addSong("Shape of You", 1);
    playlist.addSong("Blinding Lights", 2);
    playlist.addSong("Dance Monkey", 3);

    // Display the playlist
    cout << "Playlist:" << endl;
    playlist.displayPlaylist();

    // Remove a song
    playlist.removeSong(1);
    cout << "Playlist after removing song with ID 1:" << endl;
    playlist.displayPlaylist();

    return 0;
}

```
