# Trie 
A Trie data structure can be effectively used to implement song search functionalities because it allows for quick lookup and autocomplete features based on partial song titles or artist names. This efficiency is crucial for providing a smooth user experience when searching through Spotify's extensive catalog of songs.

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
# B-Trees 
