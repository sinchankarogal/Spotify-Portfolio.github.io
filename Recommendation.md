# Recomendation 

## Collaborative Filtering 

Collaborative filtering in Spotify uses users' listening history to recommend new songs based on the preferences of similar users.
By calculating the similarity between users (e.g., using cosine similarity) and identifying the nearest neighbors, Spotify can suggest songs that
these similar users have listened to but the target user hasn't yet discovered. This method leverages collective user data to provide personalized recommendations, 
enhancing the listening experience by introducing users to new music they are likely to enjoy.

![alt text](https://miro.medium.com/v2/resize:fit:1400/0*2100f4rNjGyW3AgJ.gif)


### code 

```py
import numpy as np
from sklearn.metrics.pairwise import cosine_similarity
from sklearn.neighbors import NearestNeighbors
listening_matrix = np.array([
    [5, 3, 0, 1, 0, 0],
    [4, 0, 0, 1, 0, 0],
    [1, 1, 0, 5, 0, 0],
    [0, 0, 5, 4, 0, 0],
    [0, 0, 5, 0, 0, 0],
    [0, 4, 4, 0, 0, 0],
])

# Calculate the cosine similarity between users
user_similarity = cosine_similarity(listening_matrix)

# Number of nearest neighbors to consider
k = 2

# Find the k-nearest neighbors for each user
neighbors = NearestNeighbors(n_neighbors=k, metric='cosine').fit(listening_matrix)
distances, indices = neighbors.kneighbors(listening_matrix)

# Function to recommend songs for a user
def recommend_songs(user_index, num_recommendations=2):
    user_listening = listening_matrix[user_index]
    similar_users = indices[user_index]
    recommendations = {}

    for similar_user in similar_users:
        if similar_user == user_index:
            continue
        for song_index, listens in enumerate(listening_matrix[similar_user]):
            if user_listening[song_index] == 0 and listens > 0:
                if song_index not in recommendations:
                    recommendations[song_index] = 0
                recommendations[song_index] += listens

    sorted_recommendations = sorted(recommendations.items(), key=lambda x: x[1], reverse=True)
    return [song_index for song_index, _ in sorted_recommendations[:num_recommendations]]

# Example usage: Get recommendations for user 0
user_index = 0
recommended_songs = recommend_songs(user_index)
print(f"Recommended songs for user {user_index}: {recommended_songs}")
```
