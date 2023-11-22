# lastfm2sqlsync

A simple tool that syncs scrobbling on last.fm with your own database

### Prerequisities:

- Use a tool such as https://benjaminbenben.com/lastfm-to-csv/ to make the
  initial export.

- Create a database and import the csv, map to the correct columns

```sql
-- music.scrobbles definition
CREATE TABLE `scrobbles` (
  `artist` varchar(512) DEFAULT NULL,
  `album` varchar(512) DEFAULT NULL,
  `title` varchar(512) DEFAULT NULL,
  `scrobbled_on` datetime DEFAULT NULL,
  `scrobbleid` int(11) NOT NULL AUTO_INCREMENT,
  `source` varchar(50) DEFAULT NULL,
  `epoch` int(11) DEFAULT NULL,
  PRIMARY KEY (`scrobbleid`)
) ENGINE=InnoDB AUTO_INCREMENT=616582 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;
```

- Run this tool once to fetch and import the 50 latest scrobbles. If there are
  duplicates at this stage, you can sort it out manually. In the next runs and
  in the future there will be none.

- Add the script in your crontab:

```bash
# sync lastfm scrobbles to sql db every 25 minutes
*/25 * * * * lastfm2sqlsync >/home/scp1/LASTFM_SYNC.log
```

```
Inserted scrobble: Lori McKenna - The Kitchen Tapes - Jealousy - 1700651746
Inserted scrobble: Lori McKenna - The Kitchen Tapes - Bible Song - 1700651490
Skipping duplicate scrobble: Lori McKenna - The Kitchen Tapes - How to Be Righteous - 1700651139
Skipping duplicate scrobble: Lori McKenna - The Kitchen Tapes - Falter - 1700650858
Skipping duplicate scrobble: Lori McKenna - Pieces of Me - You Are Loved - 1700650579
```
