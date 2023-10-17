# Writeup

The data is extracted from the [Ultimate National Team Database](http://hartti.com/national_teams/index.php) with the following SQL query

```
SELECT people.last_name as last_name, people.first_name as first_name, teams.division as division, COUNT(DISTINCT team_roster.tr_id) as tournaments, COUNT(teams_in_games.game_id) as games, COUNT(CASE WHEN teams_in_games.win_or_loss = "W" THEN 1 END) as wins, COUNT(CASE WHEN teams_in_games.win_or_loss = "L" THEN 1 END) as losses
FROM team_roster, teams, people, teams_in_games, games
WHERE teams.country = "fi" AND teams.team_id = team_roster.team_id AND people.person_id = team_roster.person_id AND teams_in_games.team_id = team_roster.team_id AND games.game_id = teams_in_games.game_id AND games.official = "Y"
GROUP BY teams.division, people.person_id
ORDER BY division ASC, games DESC, wins DESC, last_name ASC, first_name ASC

```