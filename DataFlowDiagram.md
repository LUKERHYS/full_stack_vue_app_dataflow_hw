# dataFlow

## newGame
1. All data is pulled from the game form using vue's v-models on a @click button.
2. The is @click also runs the addGame() method.
3. The addGame()_method then takes a game object as a payload adds a JSON header and turn the payload into a JSON string.
4. The last thing the addGame() method does is turn the entire result in to a JSON object and uses a fetch PUSH request to send the data to the back end via the API url.
5. Once sent to the backend it hits the Server.js file and gets passed through to the Create_router.js file's POST route.
6. The Create_router.js file passes the game though the POST route and assigns the JSON body of the game JSON object to the newData const.
7. As the server.js file that is handling this transaction also has a connection the the MongoDB it can use the insertOne() syntax passing in the newData const and therefore adding the new game to the games conllection.

## displayCardsInGrid
### allCardsOnStart
1. In the GameGrid.vue the mounted() method the getGame() function is called.
2. This pulls all of the presented JSON data from the base URL of the API.
3. This hits the back end of the and is directed through the GET route in the create_routers.js
4. The get route then uses the find() MongoDB method to find all the data as no argument is passed in
5. this is then puts all of the opjects into an array with the .toArray() method.
6. finally the api URL presents the result as JSON for the fron end to consume with .then((docs) => res.json(docs))
7. The JSON data is the assigned into the empty games array in the GamesGrid.vue by front end.

### newCardOnFrontEnd
1. In order to add a new card to the grid the front end does not need to access the DB at all although it does also pass a copy down to the DB as explained in the newGame data flow.
2. The @click that was used to pass the data from the form down through the addGame() method also triggers an event bus broadcast.
3. This is broadcast on the game-added frequencey and then listened for by the GamesGrid.
4. When ever the form is submitted the event bus pushes a copy into the gamesGrid games array and displays it imediatly.

## Answers 
1. The routes are defined by the create_routers.js file and used in the server.js file.
2. The folder structure is very clearly split between the fron and back end. The client folder is responsible for ever thing the user sees and interacts with where as the server folder is responsible for the dealing the the behind the senses or Backend methodsand logic, interacting with the server and database.
3. The server.js file is our 'app.js' file for the back end, it controls what packages are required for the back end to run as well handling the database connections and what ports the back end will both run and listen on. Lastly it calls the routes that are defined create_routers.js.
4. The gamesRouter is used to assign the specific routes to the generic routes in the create_router.js file. 
5. The client uses a fetch request assigned with a post method that sends data over the backend's base url 'http://localhost:3000/api/games/'
6. As mentioned above it uses a POST method rather that the default GET method normally associated with fetch.
7. The front end consumes the Default GET route called in the gameServices.js getGames() method.
8. We use the mongoDB driver to be able to consume api data from our db and preasent it as expected with JS promises and callback functionallity.


## Extension
1. We use the mongo ObjectId to target an object in the database when needing to destory or find a specific object.

### deleteGameDB
1. In the GameCard.vue the delete button is using on:click to call the deleteGame() method.
2. This method call the deleteGame() method of the the same name from the GameService.js file passing the id of the current game with the this.game.id argument syntax.
3. This deleteGame() method from the GameService.js file envokes another fetch() request this time passing the api url with the the DELETE method and the game ID appended to the end of the url.
4. This then hist the server.js file at the back end passing it through the delete route in the create_routers.js file.
5. The DELETE takes in the id passed from the front end in the url and uses the .deleteOne() mongoDB_syntax to delete the item with a matching _id object, this is passed in to the method like this { _id: ObjectID(id)}.

###deleteGameCard
1. As with the perivious explinations the addition and removal of frontend objects such as the gameCards is handled by the front end with no need to approach the database.
2. Also like with the create card, the delte card relise on an event bus transmition being triggered this time broadcasting on the game-deleted frequency.
3. The GameGrid.vue is listeing out for game-deleted to be fired with an id, this id is the compared agaist objects in the games[] array.
4. If an object id in the games array matches then the the .splice() method is called with said id passed in and the object if removed from the array.
5. the removal of the object then removes it from the gamesgrid. 


