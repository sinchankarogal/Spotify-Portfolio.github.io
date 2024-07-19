<!--# Lyrics Synchronization -->

## AVL trees 
AVL trees are used in Spotify for lyric synchronization due to their efficient balanced structure, ensuring \( O(log n) \) operations for quick retrieval and updates of lyric segments based on song playback timestamps. 
Their self-balancing nature guarantees stable performance, crucial for real-time applications like synchronized lyrics.

![alt text](https://d18l82el6cdm1i.cloudfront.net/uploads/YieLsCqeuV-avlbal.gif)
### code
```cpp
#include <iostream>
#include <algorithm> 
using namespace std;

struct AVLNode {
    int data;
    AVLNode* left;
    AVLNode* right;
    int height; 

    AVLNode(int value) : data(value), left(nullptr), right(nullptr), height(1) {}
};

int getHeight(AVLNode* node) {
    if (node == nullptr)
        return 0;
    return node->height;
}

int getBalanceFactor(AVLNode* node) {
    if (node == nullptr)
        return 0;
    return getHeight(node->left) - getHeight(node->right);
}

AVLNode* rotateRight(AVLNode* y) {
    AVLNode* x = y->left;
    AVLNode* T2 = x->right;

    x->right = y;
    y->left = T2;

    y->height = max(getHeight(y->left), getHeight(y->right)) + 1;
    x->height = max(getHeight(x->left), getHeight(x->right)) + 1;

    return x;
}

AVLNode* rotateLeft(AVLNode* x) {
    AVLNode* y = x->right;
    AVLNode* T2 = y->left;

    y->left = x;
    x->right = T2;

    x->height = max(getHeight(x->left), getHeight(x->right)) + 1;
    y->height = max(getHeight(y->left), getHeight(y->right)) + 1;

    return y;
}

AVLNode* insert(AVLNode* root, int value) {
    if (root == nullptr)
        return new AVLNode(value);

    if (value < root->data)
        root->left = insert(root->left, value);
    else if (value > root->data)
        root->right = insert(root->right, value);
    else 
        return root;

    root->height = 1 + max(getHeight(root->left), getHeight(root->right));

    int balance = getBalanceFactor(root);


    if (balance > 1 && value < root->left->data)
        return rotateRight(root);

    if (balance < -1 && value > root->right->data)
        return rotateLeft(root);

 
    if (balance > 1 && value > root->left->data) {
        root->left = rotateLeft(root->left);
        return rotateRight(root);
    }


    if (balance < -1 && value < root->right->data) {
        root->right = rotateRight(root->right);
        return rotateLeft(root);
    }

   
    return root;
}


void inorderTraversal(AVLNode* root) {
    if (root != nullptr) {
        inorderTraversal(root->left);
        cout << root->data << " ";
        inorderTraversal(root->right);
    }
}

int main() {
    AVLNode* root = nullptr;


    root = insert(root, 10);
    root = insert(root, 20);
    root = insert(root, 30);
    root = insert(root, 40);
    root = insert(root, 50);
    root = insert(root, 25);


    cout << "Inorder traversal of AVL tree:" << endl;
    inorderTraversal(root);
    cout << endl;

    return 0;
}
```
