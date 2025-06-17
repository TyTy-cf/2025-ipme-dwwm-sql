
### 1. Afficher les 9 derniers album sortis, avec le nom de l’album, le nom de l’artiste et seulement son année de sortie.

```sql
SELECT artist.name, album.name, YEAR(album.published_at)
FROM album
JOIN artist ON artist.id = album.artist_id
ORDER BY published_at DESC
LIMIT 9;
```

### 2. Afficher le nombre de playlist par compte, triez-les par ordre décroissant, sur le nombre de playlist.

```sql
SELECT account.name, COUNT(*)
FROM account
JOIN playlist ON playlist.account_id = account.id
GROUP BY account.id
ORDER BY COUNT(*) DESC;
```

### 3. Afficher par album, la durée totale de celui-ci en seconde et son nombre de chansons, on doit voir : « Nom album | Durée totale | Nombre de chansons »

```sql
SELECT album.name AS "Nom album", SUM(song.duration) AS "Durée totale", COUNT(song.id) AS "Nombre de chansons"
FROM album
JOIN album_song ON album_song.album_id = album.id
JOIN song ON song.id = album_song.song_id
GROUP BY album.id;
```

### 4. Afficher par playlist, la durée totale de celle-ci en seconde et son nombre de chansons, on doit voir : « Nom playlist | Durée totale | Nombre de chansons »

```sql
SELECT playlist.name AS "Nom playlist", SUM(song.duration) AS "Durée totale", COUNT(song.id) AS "Nombre de chansons"
FROM playlist
JOIN playlist_song ON playlist_song.playlist_id = playlist.id
JOIN song ON song.id = playlist_song.song_id
GROUP BY playlist.id;
```

### 5. Afficher le(s) nom(s) de(s) artiste(s) les plus vieux, et seulement sur l’année. Par exemple, s’il y a 2 artistes en 1976 et que c’est l’année la plus basse, je veux voir les deux (on ignore le jours et le mois).

```sql
SELECT artist.name, YEAR(artist.created_at)
FROM artist
WHERE YEAR(artist.created_at) = (
    SELECT MIN(YEAR(artist.created_at))
    FROM artist
);
```

### 6. Afficher l’âge moyen de tous les utilisateurs de l’application.

```sql
SELECT AVG(YEAR(NOW()) - YEAR(account.birth_date)) AS "Age Moyen"
FROM account;
```

### 7. Afficher le nombre d’abonnés à la newsletter (colonne newsletter = 1 de la table account).

```sql
SELECT COUNT(*)
FROM account
WHERE account.newsletter = 1;
```

### 8. Afficher le nombre d’utilisateur par genre. Bonus : labellisé les acronymes par leur signification :

• « F » = Femme

• « H » = Homme

• « NB » = Non-binaire

• « NR » = Non-renseigné

```sql
SELECT IF(account.gender = 'F', 'Femme',
          IF(account.gender = 'H', 'Homme',
             IF (account.gender = 'NB', 'Non binaire', 'Non Renseigné'))) AS "Genre",
       COUNT(*)
FROM account
GROUP BY account.gender;
```

### 9. Afficher par nom d’abonnement, le nombre de fois où il est présent sur l’année 2024.

```sql
SELECT subscription.name, COUNT(*) AS 'Nb'

FROM subscription
JOIN account_subscription AS aSub ON aSub.subscription_id = subscription.id

WHERE YEAR(aSub.effective_at) <= 2024
AND (
    aSub.finished_at IS NULL
    OR
    YEAR(aSub.finished_at) >= 2024
)

GROUP BY subscription.name;
```

### 10. Afficher les utilisateurs n’ayant jamais créer de playlists.

```sql
SELECT COUNT(*) AS "Utilisateurs sans playlist"
FROM account
WHERE account.id NOT IN (
    SELECT DISTINCT playlist.account_id
    FROM playlist
);
```

### 11. Afficher le nombre de like par playlist, avec le nom du propriétaire de la playlist, on doit voir : « Nom playlist | Nom propriétaire | Nombre de like »

```sql
SELECT playlist.name AS "Playlist", account.name AS "Utilisateur", COUNT(*) AS "Nb likes"
FROM playlist
JOIN account ON account.id = playlist.account_id
JOIN account_like_playlist ON playlist.id = account_like_playlist.playlist_id
GROUP BY playlist.id
ORDER BY COUNT(*) DESC;
```

### 12. Quelle est l’écart de durée entre la chanson la plus longue et la chanson la plus courte ?

```sql
SELECT
    DISTINCT (
         (
             SELECT MAX(song.duration)
             FROM song
         )
             -
         (
             SELECT MIN(song.duration)
             FROM song
         )
     ) AS "Ecart durée"
FROM song;
```

### 13. Afficher le nombre de chansons sorties par années, depuis les 5 dernières années

```sql
SELECT COUNT(*) AS 'Nb chansons', YEAR(album.published_at)
FROM song
JOIN album_song ON album_song.song_id = song.id
JOIN album ON album.id = album_song.album_id
WHERE YEAR(album.published_at) >= YEAR(NOW() - INTERVAL 5 YEAR)
GROUP BY YEAR(album.published_at);
```

