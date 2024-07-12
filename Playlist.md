# Playlist Management 

## Doubly Linked List 

![alt text](https://www.wikitechy.com/technology/wp-content/uploads/2017/10/doubly-linked-list.gif)

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

