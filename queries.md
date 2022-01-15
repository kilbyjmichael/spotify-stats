# Sqlite Queries Reference



#### Songs listened compared to release date

```
SELECT
Plays.ts,
Albums.release_date,
Albums.album_name,
Songs.artist_name,
Songs.name

FROM
    Plays
    INNER JOIN Songs 
	ON Songs.spotify_track_uid = Plays.spotify_track_uid
	INNER JOIN Albums 
	ON Albums.spotify_artist_uid = Songs.spotify_artist_uid
	
WHERE substr(Plays.ts,0,8)  LIKE substr(Albums.release_date,0,8)

GROUP BY 
  Songs.name, Songs.artist_name

ORDER BY 
  Songs.popularity DESC;
```