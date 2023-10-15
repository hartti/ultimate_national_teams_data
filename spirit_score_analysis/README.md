```
SELECT t1.tournament_id, t1.start_date, t1.type, t1.division, t1.country, t1.spirit_placing, t1.spirit_score, t2.participating_countries, t2.spirit_placings, t2.spirit_scores FROM
(SELECT teams.tournament_id, start_date, type, tournaments.division, country, spirit_placing, spirit_score
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