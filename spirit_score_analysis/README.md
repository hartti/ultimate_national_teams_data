# Spirit of the Game

## The data

The available data is included in this folder in SOTG_sumaries.csv -file

The data is extracted from the [Ultimate National Team Database](http://hartti.com/national_teams/index.php) with the following SQL query

```
SELECT t1.start_date, t1.type, t1.division, t2.participating_countries, t1.country, t1.placing, t1.spirit_placing, t1.spirit_score, t2.spirit_placings, t2.spirit_scores FROM
(SELECT teams.tournament_id, start_date, type, tournaments.division, country, placing, spirit_placing, spirit_score
FROM teams, tournaments, ultimate_events
WHERE teams.tournament_id = tournaments.tournament_id AND tournaments.event_id = ultimate_events.event_id AND national_team = "Y" AND spirit_placing is not null) t1
INNER JOIN 
(SELECT teams.tournament_id, participating_countries, COUNT(spirit_placing) as spirit_placings, COUNT(spirit_score) AS spirit_scores
FROM teams, tournaments
WHERE teams.tournament_id = tournaments.tournament_id
GROUP BY tournament_id  
HAVING spirit_placings > 0) t2
ON t2.tournament_id = t1.tournament_id
ORDER BY t1.start_date, t1.division, t1.spirit_placing

```

The structure of the data is following

| Column no | Column title | Description |
|-----------|--------------|-------------|
| 1 | start_date | The start date of the tournament in question in the format of yyyy-mm-dd   |
| 2 | type | The type of the tournament (wc is World Championships, ec is European Championships, aoc is Asea-Oceanic Championships, etc.) |
| 3 | div | The shorthand of the division (OPN is Open, WMN is Women, etc.) This needs to be fixed to follow the current WFDF guidelines |
| 4 | no_of_countries | How many countries participated in the tournmant in that division |
| 5 | country | The country with 2-letter ISO-code |
| 6 | placing | The final placing of teh country in the tournament |
| 7 | spirit_placing | The final spirit score placing of the country |
| 8 | spirit_score | The final spirit score for the team |
| 9 | no_of_spirit_placings | How many teams from the division have spirit placing information available (for some older tournaments, only the winner is known) |
| 10 | no_of_spirit_scores | How many teams from the division have spirit score value available |

## The spirit data averages for countries and divisions


## The correlation of teams placing and spirit placing

