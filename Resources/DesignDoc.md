# Recommendation Service Project Design Doc - Magic The Gathering Synergy Service Design


## 1. Problem Statement

[Magic: The Gathering](https://magic.wizards.com/en/intro) is a
table-top trading card game that has been around since the early 1990s. While the game 
itself is already challenging, the deck-building aspect can add another layer of complexity.
This service will provide a path to simplifying the deck-building process by offering recommendations for cards 
with high synergy. This will allow players, especially new ones, to avoid pouring through the 10s of thousands
of magic cards currently in circulation.

This design document describes the Magic The Gathering Recommendation Service, 
a new service that will provide the custom card recommendations to meet
our customers' needs. It is designed to interact with the Magic The Gathering database
and will return a list of cards that synergize with the card found in the database.

## 2. Top Questions to Resolve in Review

1. How will we design our recommender method? 
2. How will we connect to the wizards of the coast's database?
3. Do you know of any other teams out there who are working on related problems?
   Or might have the same concerns about how to create the recommender method?

## 3. Use Cases

U1. As a user, I want to input new cards as recommendations.

U2. As a user, I want to upvote/downvote cards that are already in the recommendation list.

U3. As a user, I want to retrieve a list of recommendations.

U4. As a user, I want to be able to create a profile.

U5. As a user, I want to save a searched card.

U6. As a user, I want to see the market price via TCG when I'm looking 
at a card.

U6. As a guest, I want to be able to view/search cards.

U7. As an admin/moderator, I want to delete cards from the recommendation lists.

U8. As an admin/moderator, I want to delete or update users.

U9. As an admin, I want to assign moderators.

## 4. Project Scope

### 4.1. In Scope

* Adding cards to another card's recommendations list.
* Up-voting and Down-voting cards within a recommendations list.
* Storing favorite cards within a profile.
* Searching for card based on the API requirements.

### 4.2. Out of Scope

* Creating a Mtg game platform.
* Adding in a deck-builder rules and possible limitations.
* Adding in machine learning recommendation systems.
* Adding in message boards related to cards.
* Look into how this would work as a plug-in for WOTC.

## 5. Proposed Architecture Overview

This initial iteration will provide the minimum viable product (MVP) including
Adding and retrieving cards to and from a recommendation list, as well as adding to a profile.
It will also include Up-voting and Down-voting of cards in the list to help out the
recommendation method in sorting cards.

We will use SpringBoot and an SQL Database to create _____ endpoints ()
that will handle the retrieval of cards and the addition of said cards to the list to satisfy our
requirements.

We will store cards for our profile in a table in a Recommendations list. Recommendation lists
themselves will be stored in an SQL database.

RecommendationService will also provide a web interface for users to manage
their favorite cards. A main page providing a list view of all of their decklists
will let them select a deck to view and add more cards to the list.

## 6. API

### 6.1. Public Models

```
// UserModel

String userId;
String userName;
String role;
String profileImageUrl
List<List<String, String>> recommendationList; 
//Possibly be a LinkedList

```

```
// CardModel
String cardName;
String cardImageUrl;
String cardId;
String colors;
String colorIdentity;
String types;
String subTypes;
// Many more query parameters are available that could 
// be added at later date
```

### 6.2. Get Card Endpoint

* Accepts `GET` requests to `/Cards/:pathparameter`
* Accepts a cards's name and returns the corresponding CardModel.
    * If the given card is not found, will throw a
      `CardNotFoundException`

### 6.3. Create User Endpoint

* Accepts `POST` requests to `/users`
* Accepts data to create a new user with a provided userName, a given userId
  ID, and an List of Recommendation lists. Returns the new user.

### 6.4. Update Recommendations List Endpoint

* Accepts `PUT` requests to `/cards/:id`
* Accepts data to update a recommendation list including a card's ID and the card to be added's ID 
associated with the playlist. Returns the updated playlist.

### Update Profile List Endpoint

* Accepts 'PUT' requests to '/user/:listId'

### 6.5. Get User Endpoint

* Accepts `GET` requests to `/user/:id`
* Accepts a user ID and returns the user.
    * If the user is not found, will throw a `UserNotFoundException'
    
    
## 7. Tables

### 7.1. `user`

```
id // partition key, string
userName // string
email // string
imageUrl // string
recommendationLists // list<list<String,String>>
```

### 7.2. `card`

```
 cardName // string
 cardImageUrl // string
 cardId // partition key, string
 colors // string
 colorIdentity // string
 types // string
 subTypes // string
```

## 8. Pages

![Clicking on a playlist name on the home page opens the view playlist page.
Clicking the plus icon on the homepage opens the create playlist page. Clicking
the Create button on the create playlist page opens the view playlist page.
Clicking the plus icon on the view playlist page opens the add song page.
Clicking add on the Add Song page opens the view playlist
page.](images/example_design_document/OverallWorkflow.png)

![The homepage has a header that reads "Amazon Playlists." It also displays the
user's alias. Each page has this header. Each playlist the user has is shown as
a box with the playlist name and any tags it has. There is also a plus
icon.](images/example_design_document/Homepage.png)

![The create playlist page says "Create Playlist" with a field for name and
tags. There is a "Create"
button.](images/example_design_document/CreatePlaylist.png)

![The view playlist page has the name of the playlist and any tags associated
with it. Each song in the playlist is listed in order. There is a plus button
to add a new song to the
playlist.](images/example_design_document/ViewPlaylist.png)

![The add song page has a form titled "Add Song." It displays the playlist name
followed by fields for the asin and track number. THere is a check box for
queue next. Finally, there is an "Add" button to submit the
form.](images/example_design_document/AddSong.png)
