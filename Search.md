# Search

### Trie 
A Trie data structure can be effectively used to implement song search functionalities because it allows for quick lookup and autocomplete features based on partial song titles or artist names. This efficiency is crucial for providing a smooth user experience when searching through Spotify’s extensive catalog of songs.

![alt text](https://miro.medium.com/v2/resize:fit:1400/1*aJxRGNYe52CE_bVRt0E1Eg.gif)


##  code

```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
using namespace std;

class TrieNode {
public:
    unordered_map<char, TrieNode*> children;
    bool is_end_of_word;
    vector<int> song_ids; 
    TrieNode() {
        is_end_of_word = false;
    }
};

class Trie {
public:
    TrieNode* root;
    Trie() {
        root = new TrieNode();
    }
    void insert(string title, int song_id) {
        TrieNode* node = root;
        for (char ch : title) {
            if (node->children.find(ch) == node->children.end()) {
                node->children[ch] = new TrieNode();
            }
            node = node->children[ch];
        }
        node->is_end_of_word = true;
        node->song_ids.push_back(song_id);
    }
    ]
    vector<int> search(string prefix) {
        TrieNode* node = root;
        for (char ch : prefix) {
            if (node->children.find(ch) == node->children.end()) {
                return {}; // Return empty vector if prefix not found
            }
            node = node->children[ch];
        }
        return node->song_ids;
    }
};


int main() {
    Trie trie;
    trie.insert("Shape of You", 1);
    trie.insert("Blinding Lights", 2);
    trie.insert("Dance Monkey", 3);
    trie.insert("Rockstar", 4);
    trie.insert("Circles", 5);
    trie.insert("Someone Like You", 6);
    trie.insert("Despacito", 7);
    string prefix = "D";
    vector<int> result = trie.search(prefix);
    if (result.empty()) {
        cout << "No songs found with the prefix '" << prefix << "'." << endl;
    } else {
        cout << "Songs found with prefix '" << prefix << "':" << endl;
        for (int song_id : result) {
            cout << "Song ID: " << song_id << endl;
        }
    }
    return 0;
}

```

## Soundex Algorithm 
The Soundex algorithm can be used in Spotify to improve the search functionality by matching phonetically similar song titles. It encodes words into a standardized format based on their pronunciation, allowing users to find songs even if the spelling is slightly different. This is particularly useful for handling misspellings or variations in titles, ensuring that users can still find the correct songs with minimal input errors.

![alt text](https://ars.els-cdn.com/content/image/3-s2.0-B978012800537800003X-f03-04-9780128005378.jpg)

### Code

```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
#include <string>
#include <cctype>

using namespace std;

string getSoundex(const string& s) {
    // Soundex encoding map
    unordered_map<char, char> soundexMap = {
        {'B', '1'}, {'F', '1'}, {'P', '1'}, {'V', '1'},
        {'C', '2'}, {'G', '2'}, {'J', '2'}, {'K', '2'}, {'Q', '2'}, {'S', '2'}, {'X', '2'}, {'Z', '2'},
        {'D', '3'}, {'T', '3'},
        {'L', '4'},
        {'M', '5'}, {'N', '5'},
        {'R', '6'}
    };
    
    string result;
    if (s.empty()) return result;
    
    // Convert the first letter to uppercase and add it to the result
    result += toupper(s[0]);
    
    // Encode the rest of the letters
    char lastCode = soundexMap[result[0]];
    for (size_t i = 1; i < s.size(); ++i) {
        char c = toupper(s[i]);
        if (soundexMap.count(c) > 0) {
            char code = soundexMap[c];
            if (code != lastCode) {
                result += code;
                lastCode = code;
            }
        }
    }
    
    // Pad with zeros or trim to ensure the result is 4 characters long
    result = result.substr(0, 4);
    while (result.length() < 4) {
        result += '0';
    }
    
    return result;
}

class Song {
public:
    int id;
    string title;
    string soundexCode;

    Song(int id, const string& title) : id(id), title(title) {
        soundexCode = getSoundex(title);
    }
};

class MusicLibrary {
public:
    vector<Song> songs;
    
    void addSong(int id, const string& title) {
        songs.emplace_back(id, title);
    }

    vector<int> searchSongs(const string& query) {
        string querySoundex = getSoundex(query);
        vector<int> result;
        for (const auto& song : songs) {
            if (song.soundexCode == querySoundex) {
                result.push_back(song.id);
            }
        }
        return result;
    }
};

int main() {
    MusicLibrary library;
    library.addSong(1, "Blinding Lights");
    library.addSong(2, "Blinding Lites");
    library.addSong(3, "Bliding Lights");
    library.addSong(4, "Dancing Queen");
    library.addSong(5, "Bohemian Rhapsody");

    string query = "Blinding Lites";
    vector<int> searchResults = library.searchSongs(query);

    if (searchResults.empty()) {
        cout << "No songs found for the query '" << query << "'." << endl;
    } else {
        cout << "Songs found for the query '" << query << "':" << endl;
        for (int id : searchResults) {
            cout << "Song ID: " << id << endl;
        }
    }

    return 0;
}

```
# B-Trees 
