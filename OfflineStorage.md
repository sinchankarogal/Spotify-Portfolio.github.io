# Efficient Offline Storage 

## Hauffman Coding 

Huffman coding compresses audio files and metadata, significantly reducing the storage space needed for offline songs on user devices. 
This allows users to store more songs offline without requiring additional storage. Additionally, it optimizes bandwidth usage by reducing the size of data during transmission,
leading to faster downloads and more efficient space management, thereby enhancing the overall user experience with offline listening.

![alt text](https://upload.wikimedia.org/wikipedia/commons/a/ac/Huffman_huff_demo.gif)


### Code 
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <unordered_map>
using namespace std;

// Node class to represent each character and its frequency
class Node {
public:
    char ch;
    int freq;
    Node *left, *right;

    Node(char ch, int freq) {
        this->ch = ch;
        this->freq = freq;
        left = right = nullptr;
    }
};

// Custom comparator for priority queue
class Compare {
public:
    bool operator()(Node* left, Node* right) {
        return left->freq > right->freq;
    }
};

// Function to encode the characters and store the codes in a map
void encode(Node* root, string str, unordered_map<char, string> &huffmanCode) {
    if (root == nullptr) {
        return;
    }
    if (!root->left && !root->right) {
        huffmanCode[root->ch] = str;
    }
    encode(root->left, str + "0", huffmanCode);
    encode(root->right, str + "1", huffmanCode);
}

// Function to decode the encoded string
void decode(Node* root, int &index, string str) {
    if (root == nullptr) {
        return;
    }
    if (!root->left && !root->right) {
        cout << root->ch;
        return;
    }
    index++;
    if (str[index] == '0') {
        decode(root->left, index, str);
    } else {
        decode(root->right, index, str);
    }
}

// Function to build the Huffman Tree and encode the text
void buildHuffmanTree(string text) {
    unordered_map<char, int> freq;
    for (char ch : text) {
        freq[ch]++;
    }

    priority_queue<Node*, vector<Node*>, Compare> pq;

    for (auto pair : freq) {
        pq.push(new Node(pair.first, pair.second));
    }

    while (pq.size() != 1) {
        Node* left = pq.top(); pq.pop();
        Node* right = pq.top(); pq.pop();
        int sum = left->freq + right->freq;
        pq.push(new Node('\0', sum));
    }

    Node* root = pq.top();

    unordered_map<char, string> huffmanCode;
    encode(root, "", huffmanCode);

    cout << "Huffman Codes:\n";
    for (auto pair : huffmanCode) {
        cout << pair.first << " " << pair.second << "\n";
    }

    cout << "\nOriginal string:\n" << text << "\n";

    string str = "";
    for (char ch : text) {
        str += huffmanCode[ch];
    }

    cout << "\nEncoded string:\n" << str << "\n";

    int index = -1;
    cout << "\nDecoded string:\n";
    while (index < (int)str.size() - 2) {
        decode(root, index, str);
    }
}

int main() {
    string text = "spotify music compression example";
    buildHuffmanTree(text);
    return 0;
}
```
