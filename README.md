# Teori Graf

Kode notebook untuk hasil project akhir Teori Graf - Group 5

### Recommender System Based On Common Followings and User Activities

Understanding
------------------------------------------------------------------------------------------
Common Followings: 
- User A follows User B1, B2, ..., Bn
- User B1, B2, ..., Bn follow User C
- User A is heavily recommended to follow User C

User Activities: (Target = Potential Accounts to be recommended for User to follow)
- Latest Target's activity date (latest Post date / Account creation date)
- User's interaction with Target
  - User commenting under the Target's Post
  - Target commenting under the User's Post
- Target's number of Posts to determine how active the Target is

SubFunctions
------------------------------------------------------------------------------------------
- get_weight
  - Calculate the User's initial weight based on its latest post date
  - weight = 1                      , if days_since_last_post is <= 3
  - weight = 1 / (days - 3)**(1/3)  , if days_since_last_post is > 3
  - input: User (Account object)
  - output: weight
- user_activity_value
  - Giving additional weights for the target based on the number of posts made by the Target
  - input: Target (Account object), weight (of the target)
  - output: Target (Account object), new weight
- interaction_value
  - Check interactions between user Account and target Account then give numerical value to it.
  - If Target commented on a User's post, then the weight is increased by 0.02 / comment
  - If User commented on a Target's post, then the weight is increased by 0.05 / comment
  - input: User (Account object), Target (Account object), weight (of the target)
  - output: Target (Account object), new weight


General Algorithm
------------------------------------------------------------------------------------------
1. Initiate following_potential as dictionary data type to store all the Targets to be recommended to User
2. Check all the User's followings, each as first_search
3.  Check all the first_search's followings, each as second_search
4.   For each second_search, check whether the second_search is the User itself or already followed by the User, and if so, continue to another second_search
5.   Get the initial weight for the second_search with get_weight function
6.   If the second_search is already in the following_potential then multiply its existing weight by 1.2 else add it to the dict with its initial weight
7. After the following_potential is populated, then for each target in there calculate its:
8.  Activity value based on the Target's number of Posts.
9.  Interaction value with the User
10. Sort the following_potential by descending order
