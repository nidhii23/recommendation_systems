
### **Movie Recommendation System Report**  
**Using Collaborative Filtering and Random Walks (Pixie-Inspired)**

---

### **1. Introduction**

There is an infinite amount of entertainment available to us in the current digital era, including Netflix, Hulu, and Prime Video.   **Recommendation systems** are useful in this situation.  These technologies operate in the background, recommends you  films that you might enjoy but otherwise would not have discovered.

 **Connect people to the correct content at the right time** is the objective of a recommendation system.


 **Collaborative filtering based on user preferences**: Suggests films that individuals with similar likes enjoyed.
 - **Item-based collaborative filtering**: Makes recommendations for films that are comparable to those you have already liked.

- **Random-walk-based (Pixie-inspired)**: Uses a graph-based exploration strategy to find movies connected to your past activity.

we have implemented all three methods
---

### **2. Dataset Description: Exploring MovieLens 100K**

To train and test our recommendation models, we used the **MovieLens 100K dataset**. It contains:

-  **943 users**
-  **1,682 movies**
- **100,000 ratings**

Each record includes a user ID, a movie ID, a rating (from 1 to 5), and a timestamp. 

**Preprocessing Steps:**
- Cleaned and removed duplicates.
- Created a **user-item rating matrix**, which maps who rated what and how.
- Built index maps for user and movie IDs to help with faster computation.
- For the graph-based approach, we also created an **adjacency list** linking users and movies based on ratings.


### **3. Methodology: How We Built the Recommenders**

We implemented three distinct recommendation strategies.
---

#### **A. User-Based Collaborative Filtering**

This method finds users who rate movies similarly and recommends what those “neighbors” liked.

- We calculated **cosine similarity** between users based on their rating vectors.
- For a target user, we selected the **top-k most similar users** and averaged their ratings to give recommendations.
- It works well when users have common preferences, but struggles with cold-start users (i.e., users with very few ratings).

---

####  **B. Item-Based Collaborative Filtering**

In this algorithm we find movies similar to those the user has already rated highly.

- We used **cosine similarity**, this time between **item vectors** (how each movie was rated by all users).
- If a user loved "The Matrix," the system might suggest "Inception" or "Blade Runner"—movies rated similarly by others.
- This method tends to be more stable over time and is great when item metadata is limited.

---

#### **C. Pixie-Inspired Random Walk Algorithm**

 Inspired by **Pixie**, Pinterest’s real-time recommendation engine, this method treats users and movies as nodes in a **bipartite graph**.

- Users are connected to the movies they've rated.
- From a **seed movie** (e.g., something you just watched), we perform multiple **random walks**.
- At each step, the walk randomly hops to connected nodes. Occasionally, it resets to the seed node.
- Movies that are visited often in these walks are likely to be relevant to interests.

This approach doesn’t rely on exact similarity but instead **explores the "neighborhood" of your interests**. It’s fast, scalable, and adapts well to fresh content and changing tastes.

---

### **4. Implementation Details**



####  **Collaborative Filtering (User/Item)**:
- Built rating vectors from the cleaned matrix.
- Used vector operations to compute similarity scores.
- Aggregated ratings from similar users/items for recommendation.

####  **Graph Construction for Random Walk**:
- Created an **adjacency list**: each user links to the movies they rated, and each movie links back to the users who rated it.
- This structure allowed us to perform fast, memory-efficient random walks.

####  **Random Walks**:
- For each walk, we started at a seed movie and walked for a fixed number of steps (e.g., 100).
- Used a simple strategy: randomly choose the next connected node.
- Tracked the frequency of visited movies to build a ranked list of recommendations.


---

### **5. Results and Evaluation **

####  **Sample Outputs**:

- **User-Based CF**: Suggested popular movies among like-minded users (e.g., "Toy Story" → "Aladdin").
- **Item-Based CF**: Returned movies with similar viewer ratings (e.g., "Star Wars" → "Empire Strikes Back").
- **Random Walk**: Returned a mix of related and serendipitous results (e.g., "Shawshank Redemption" → lesser-known but thematically similar dramas).

####  **Comparison**:

| Method              | Pros                                      | Cons                                   |
|---------------------|-------------------------------------------|----------------------------------------|
| User-Based CF       | Intuitive, personal                       | Weak for new users or sparse data      |
| Item-Based CF       | Stable, efficient                         | Doesn’t capture evolving preferences   |
| Pixie (Random Walk) | Fast, real-time, flexible                 | Quality depends on graph richness      |

####  **Limitations**:
- Random walks used uniform probabilities; no edge weights or context-based tuning.
- Lacked content features like genre or keywords.
- No personalization in walk biases (e.g., recent activity, time decay).

Still, even in this simple form, the Pixie method performed surprisingly well, especially in generating **diverse recommendations**.

---

### **6. Conclusion**

This project gave us a hands-on understanding of how recommendation systems are used and build.

- **Collaborative filtering** remains a solid choice when historical data is rich and overlapping.
- **Pixie-inspired algorithms** shine in real-time, large-scale environments where data is sparse, dynamic, or noisy.
- Graphs allow us to **move beyond direct similarity** and discover hidden relationships.

####  **Future Improvements**:
- Introduce **edge weighting** based on rating strength or recency.


#### **Real-World Application**:
These models are used by platforms like **Netflix, YouTube, Amazon, LinkedIn, and Pinterest**. Whether you're watching movies, shopping, or networking, there's likely a recommendation engine powered by some form of collaborative filtering or random walk.

### **Insights**
Meaningful Insights from the Recommendation System’s Output
1. User-Based Collaborative Filtering : Peer behaviour informs highly customised recommendations.
 The system successfully suggests films that User B enjoyed but User A hasn't seen yet if User A and User B share comparable tastes.
Functions well for particular tastes; for instance, if a user base often likes independent or foreign films, recommendations are made specifically for those who share such tastes.
An example of an output insight
Because both User 234 and User 312 like science fiction films, algorithm suggests "Blade Runner" and "The Matrix" to User 234 in light of User 312's positive reviews.
 Meaning:shows that User based collaborative filtering has the ability to identify taste communities and provide recommendations with them.Less diversity in recommendations due to overfitting to a user's group.

2. Item-Based Collaborative Filtering  : Produces steady, high-confidence suggestions by emphasising item similarity over user similarity.
 A user who gives "The Godfather" a high rating would be suggested "Goodfellas" or "Scarface," which are films that other users frequently give similar ratings.
Instead of reflecting individual behaviour, it reflects global tendencies.

 An example of an output insight
Pulp Fiction was liked by user 78.  Both "Reservoir Dogs" and "Kill Bill," which are comparable in genre and director, were suggested by algorithm.
Meaning:For new users with only a few ratings, IBCF often recommends contextually related films.
Recommendations tend to stick to similar genres and styles, therefore they might not be very generalisable for people with different likes.

3. Pixie Algorithm Based on Random Walks
 Understanding: Identifies latent and coincidental connections that conventional approaches might overlook.Random walks follow network connections that resemble natural discovery (e.g., a recommendation for a movie from a friend).frequently yields a varied list of recommendations that combines well-liked and obscure options.

 An example of an output insight
Beginning with "Titanic," Pixie suggested "Forrest Gump," "The Notebook," and "Shawshank Redemption"—all of which are not genre-specific but are connected by the actions of other users.Meaning:reflects the way actual users navigate between genres or themes. provides more exploratory suggestions, which is beneficial for platforms that find content.
