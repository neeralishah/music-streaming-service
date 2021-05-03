# Small website for music-streaming-service

A website that is backed by a sqlite database and hosted on glitch.  
Visit the website here: https://cs-348-project.glitch.me/

For the CS 348 Semester Project.

**Working Features**
* Adding Users to the service
* Searching for playlists and see who created them
* Search for what songs another user has listened to
* Search for songs by a specfic artist
* Update user account type from Standard to Premium and vice versa
* See website stats like number of user, artists, and the top song on the website!

**SQL Related Features**
* Relational database using sqlite3 - 10+ tables
* Indexes on certain columns to support faster searching
* Concurrency control for update and insert queries

**Database Schema**
Key: table_name(PK, FK, attribute_1, attritube_2,... etc)

Artist( id, name, genre)
User(username, first, last, date_joined, account_type)
Account(account_type, description)
Song(id, title, length, genre)
Album(id, title, num_songs, total_time, genre)
album_song_rel(album_id, song_id)
album_artist_rel(album_id, artist_id)
Playlist(id, name, date_created)
playlist_song_rel(song_id, playlist_id)
playlist_user_rel(username, playlist_id)
listened_to(username,song_id)

**ORM & Data Entry**
We used the Node ORM, sequelize, to initialize the tables and fields for our sqlite database.
Then, we used the .build() and .save() functions to create objects and save them to their corresponding tables.

**Prepared Statements**
Application involves multiple search and update statements.
1. HTML form (in index.html)
2. Client types something in a search field and hits submit
3. Javascript passes the object (from client.js) to an app.get statement in server.js
4. SQL query is executed using the passed object from the client in the SQLite database - query result changes on the passed value 
returns results of query to client.js 
5. client.js builds a table in index.html with the results of that search that is displayed to the user.

**Transactions**
No read or write locks happen except within the transaction. The three different types of transactions are deferred, immediate, and exclusive. When a user writes to the table, we want to use the exclusive transaction so that we can avoid phantom data and start a new write immediately while also preventing other database connections. The transaction in SQLite is defaulted to deferred, which means the transaction does not start until the database is first accessed, but in some instances we changed it so we can avoid phantom data. 

**Isolation Levels**
Our project is serializable because the default isolation level in SQLite is serializable. This is beneficial because we do not have to worry about phantom data. The read uncommitted pragma is set to off by default in SQLite but once the application turns on we have to turn it on to allow for faster results. For example, in the website stats query we turn on the read uncommitted pragma because the query works with multiple tables. To get the output to the users quicker, we set the pragma to 1 indicating that it is on.  




