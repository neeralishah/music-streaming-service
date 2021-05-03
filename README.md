# music-streaming-service

Visit the website here: https://cs-348-project.glitch.me/

Used SQLite for backend, Javascript for Server/Client, HTML for front end. 

Working Features

Adding Users to the service
Searching for playlists and see who created them
Search for what songs another user has listened to
Search for songs by a specfic artist
Update user account type from Standard to Premium and vice versa
See website stats like number of user, artists, and the top song on the website!

SQL Related Features

Relational database using sqlite3 - 10+ tables
Usage of prepared statements in JS for search queries
Indexes on certain columns to support faster searching
Concurrency control, Isolation Levels, and Transaction Types on queries
ORM for creation of tables using the 'sequelize' package

Explanation of Indexes

ind_one [CREATE INDEX IF NOT EXISTS ind_one ON Playlists(name)] supports the Playlists query and makes the process faster to find the name of the playlist when we search the table playlist later. We kept only one column because when we run search playlist our where clause indicates that we are searching for a specific name, so we made the index to only have one column to make the search as fast as possible.

The query that benefits from this index is:
to get playlist by name: select u.firstname, p.name, p.datecreated from Playlists p join playlist_user_rel purel on p.id=purel.playlist_id join Users u on purel.user_id=u.id where name='${cleansedname}'

ind_two [CREATE INDEX IF NOT EXISTS ind_two ON Artists(name)] supports the Artists query and makes it quicker to find the name of the artist when we search the Artist table for queries involving the artist name.

The queries that benefit from this index are:
to get songs by artist: `select s.title, s.genre from Songs s join album_song_rel asrel on s.id=asrel.song_id join Albums al on asrel.album_id=al.id join album_artist_rel aarel on al.id=aarel.album_id join Artists ar on ar.id=aarel.artist_id where ar.name='${cleansedartist}'`
to get website stats: select (select count(u.id) from Users u) as user, (select count(a.id) from Artists a) as artist, (select count(p.id) from Playlists p) as playlist, (select s.title from Songs s where s.id=(select song_id from (select song_id, count(*) as cnt from listened_to group by song_id order by count(*) desc limit 1) x)) as top_song`

ind_three [CREATE INDEX IF NOT EXISTS ind_three ON Users(id)] supports the Users query, when we are searching for users by the ID entered by the user on the website, we use this index to make the search faster.

The query that benefits from this table is:
to get songs by username: select s.title, s.genre from Songs s join listened_to lt on s.id=lt.song_id join Users u on lt.user_id=u.id where u.id='${cleansedId}'. 

ind_four [CREATE INDEX IF NOT EXISTS ind_four ON Songs(id)] supports the Songs and speeds up the process when we want to find the name of the song.

The query that benefits from this index is:
to get songs by artist: `select s.title, s.genre from Songs s join album_song_rel asrel on s.id=asrel.song_id join Albums al on asrel.album_id=al.id join album_artist_rel aarel on al.id=aarel.album_id join Artists ar on ar.id=aarel.artist_id where ar.name='${cleansedartist}'`
to get songs by username: `select s.title, s.genre from Songs s join listened_to lt on s.id=lt.song_id join Users u on lt.user_id=u.id where u.id='${cleansedId}'`

