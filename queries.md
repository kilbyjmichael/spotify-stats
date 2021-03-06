# Sqlite Queries Reference

### Total Play Count

```
SELECT
Plays.master_metadata_track_name,
Plays.master_metadata_album_artist_name,
Plays.master_metadata_album_album_name

FROM
    Plays


WHERE Plays.ms_played >= 20000

ORDER BY master_metadata_track_name;
```



### Unique X Count

```
SELECT
Albums.album_name,
Songs.artist_name,
Songs.name

FROM
    Plays
    INNER JOIN Songs 
	ON Songs.spotify_track_uid = Plays.spotify_track_uid
	INNER JOIN Albums 
	ON Albums.spotify_artist_uid = Songs.spotify_artist_uid

WHERE Plays.ms_played >= 20000


GROUP BY 
  Songs.name, Songs.artist_name;
```  
 

### Songs listened compared to release date

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
	
WHERE substr(Plays.ts,0,8) LIKE substr(Albums.release_date,0,8) AND Plays.ms_played >= 20000

GROUP BY 
  Songs.name, Songs.artist_name;
```



### Songs from albums by date

```
SELECT
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
	
WHERE substr(Albums.release_date,0,11)  > '2018-12-16%' AND Plays.ms_played >= 20000

GROUP BY 
  Songs.name, Songs.artist_name
  

ORDER BY 
  Albums.release_date DESC;
```

### Songs from albums by date

`SELECT SUM(Plays.ms_played) FROM Plays;`

`SELECT SUM(Plays.ms_played) FROM Plays WHERE Plays.ms_played >= 20000;`

### Songs over x% complete
```
SELECT
Plays.ts,
Plays.master_metadata_track_name,
Plays.master_metadata_album_artist_name,
Plays.ms_played,
(Songs.duration_ms - (Songs.duration_ms * .05)) AS '95% completion',
Songs.duration_ms

FROM
    Plays
    INNER JOIN Songs 
	ON Songs.spotify_track_uid = Plays.spotify_track_uid

WHERE Plays.ms_played >= (Songs.duration_ms - (Songs.duration_ms * .05))

ORDER BY ts;
```

### Platform Counting
```
SELECT count(case when platform like 'android%' then 1 end) as "Android",
       count(case when platform like '%desktop%' then 1 end) as "Desktop",
       count(case when platform like 'Partner google cast_voice%' then 1 end) as "Google Home",
	   count(case when platform not like 'android%' and platform not like '%desktop%' and platform not like 'Partner google cast_voice%' then 1 end) as "Other"
FROM Plays

WHERE Plays.ms_played >= 20000;
```

### Timestamp Counting
```
SELECT count(case when ts like '2018%' then 1 end) as "2018",
       count(case when ts like '2019%' then 1 end) as "2019",
       count(case when ts like '2020%' then 1 end) as "2020",
	   count(case when ts like '2021%' then 1 end) as "2021",
	   count(case when ts not like '2021%' and ts not like '2020%' and ts not like '2019%' and ts not like '2018%' then 1 end) as "Err Check"

FROM Plays
  
WHERE Plays.ms_played >= 20000;
```

