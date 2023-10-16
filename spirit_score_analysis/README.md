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

|div|G17|G19|G23|GMM|J17|J19|M17|M19|M23|MAS|MIX|MMX|OPN|U23|WMN|WMS|Grand Total|
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
|country|normalized_spirit_placing (Average)|||||||||||||||||
|ae|||||||||||27.36||||||27.36|
|at|1.00|18.43|12.68||8.33|15.29|1.00||10.90||46.92||59.06|39.81|11.02||22.97|
|au||27.92|29.48|||31.72|||60.67|65.08|47.92||27.08|62.06|37.71|75.25|44.72|
|be|20.80|36.36|46.00||62.88|46.75|50.50|34.00||38.13|22.12||61.16|77.10|56.98||48.53|
|br|||||||||||0.00||||||0.00|
|by|||||||||||17.50||||||17.50|
|ca||65.23|56.76|||38.58|||81.98|87.63|80.74||54.20|58.51|34.00|80.20|62.88|
|ch||37.18|28.00||42.25|46.10||17.50|53.80|100.00|14.21||56.93|52.89|81.54|17.50|45.87|
|cn||42.25|100.00|||35.94|||50.75||60.10||74.68|38.83|17.50||53.39|
|co|15.14|24.16|73.04||21.35|42.33||67.00|50.19||61.11||94.50|51.38|89.69||54.07|


## The correlation of teams placing and spirit placing

## To-do

* division acronyms should follow WFDF guidelines
* include normalized placings column and normalized spirit placing column

