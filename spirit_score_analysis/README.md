# Spirit of the Game -data

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
| 1 | start_date |    |
| 2 | type |    |
| 3 | div |    |
| 4 | no_of_countrues |    |
| 5 | country |    |
| 6 | placing |    |
| 7 | spirit_placing |    |
| 8 | spirit_score |    |
| 9 | no_of_spirit_placings |    |
| 10 | no_of_spirit_scores |    |

