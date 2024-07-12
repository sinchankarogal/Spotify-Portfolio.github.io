# Collaborative playlist 

## BFS 


BFS (Breadth-First Search) is ideal for sharing songs on Spotify because it systematically explores all immediate connections (friends or followers) of a 
user before moving on to the next level of connections. This ensures that the collaborative playlist remains up-to-date for all users involved, maintaining consistency and enhancing the collaborative experience
. By propagating updates efficiently, BFS helps keep everyone in sync with the latest additions to the playlist.

![alt text](https://upload.wikimedia.org/wikipedia/commons/5/5d/Breadth-First-Search-Algorithm.gif)


### code 

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <unordered_map>
#include <unordered_set>

using namespace std;

class SocialGraph {
public:
    unordered_map<int, vector<int>> adjList;

    void addUser(int user) {
        adjList[user] = vector<int>();
    }

    void addConnection(int user1, int user2) {
        adjList[user1].push_back(user2);
        adjList[user2].push_back(user1);
    }

    vector<int> bfsShare(int startUser, int levels) {
        vector<int> result;
        unordered_set<int> visited;
        queue<pair<int, int>> q; // Pair of (user, current level)
        
        q.push({startUser, 0});
        visited.insert(startUser);
        
        while (!q.empty()) {
            int user = q.front().first;
            int level = q.front().second;
            q.pop();

            if (level > levels) break;

            result.push_back(user);
            for (int neighbor : adjList[user]) {
                if (visited.find(neighbor) == visited.end()) {
                    q.push({neighbor, level + 1});
                    visited.insert(neighbor);
                }
            }
        }

        return result;
    }
};

int main() {
    SocialGraph sg;
    sg.addUser(1);
    sg.addUser(2);
    sg.addUser(3);
    sg.addUser(4);
    sg.addUser(5);

    sg.addConnection(1, 2);
    sg.addConnection(1, 3);
    sg.addConnection(2, 4);
    sg.addConnection(3, 5);

    vector<int> sharedUsers = sg.bfsShare(1, 2); // Share with users within 2 levels

    cout << "Users to share the song with: ";
    for (int user : sharedUsers) {
        cout << user << " ";
    }

    return 0;
}
```
