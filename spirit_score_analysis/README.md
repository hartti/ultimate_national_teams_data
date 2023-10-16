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

The following pivot table provides an overview of the "normalized" spirit placings of countries across different divisions. The normalizing here means that as there are varying number of teams in each tournament and division, the normalized spirit placings are always between 1 and 100 (1 meaning the spirit winner and 100 that the team placed last in teh spirit rankings). The table omits all the spirit placings from tournaments, where only information about the winner is available.

in more technical terms the normalized spirit placings are calculated with the following formula

```
normalized_spirit_placing = (spirit_placing -1) / (number_of_teams-1) * 99 + 1
```

|div|G17|G19|G23|GMM|J17|J19|M17|M19|M23|MAS|MIX|MMX|OPN|U23|WMN|WMS|Grand Total|
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
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
|cz|40.60|59.38|||64.53|39.93|||65.35|1.00|72.77||42.98||36.36|40.60|49.29|
|de|55.80|25.85|69.84|1.00|68.03|46.04|||24.67|39.89|8.77|85.86|57.84|65.00|29.20|14.20|42.34|
|dk||||||34.79|||8.62|25.75|41.73||45.86|19.24|14.75||31.05|
|do||||||53.41||||||||85.86|||69.63|
|eg|||||||||||||31.72||||31.72|
|es||||100.00||27.29|||60.40|71.81|72.69|15.14|62.84|47.89|75.25|60.40|58.40|
|fi|0.00|68.82|||25.75|47.99|||34.00|31.94|28.57||12.93||68.10|45.55|40.19|
|fr|100.00|65.21||67.00|71.33|67.03||67.00|70.30|45.56|58.29|71.71|30.85|36.36|56.79|55.45|62.17|
|gb|44.61|52.89|28.96||45.07|80.28|||61.10|51.38|55.23|1.00|40.73|58.34|40.45|15.85|48.49|
|hk||||||76.71|||47.62||47.76||78.00||62.88||60.42|
|hu||||||100.00|100.00|100.00|||83.50||||||94.50|
|ie||18.02|35.62||10.35|42.33||1.00|59.65||31.37||36.30|22.91|37.29||31.52|
|il||64.25|||63.79|67.91||17.50|||38.55||67.90||||60.81|
|in||||||50.50|||39.08|19.56|44.24||43.67||1.00||36.12|
|it|58.51|68.58|79.58|34.00|61.09|73.96|75.25|83.50|20.80|44.31|96.91||78.79|84.09|93.52|100.00|72.53|
|jp||85.43|74.15|||39.68|||45.66|46.08|73.54|0.00|46.56|40.63|54.08|75.25|57.30|
|kr|||||||||||63.46||||||63.46|
|lt|||||||||||91.06||93.17||||91.77|
|lv||89.00||||87.45|||90.10||62.88||57.78||||76.06|
|mx||58.75|34.00|||51.02|||25.75||55.62||33.81|30.96|52.15||42.50|
|my|||||||||100.00||1.00||17.50||1.00||29.88|
|nl|29.99|56.75|||40.60|32.88||50.50|30.70||50.50||17.11|35.43|20.33|100.00|36.01|
|no|||||||||||19.88||||||19.88|
|nz||17.07|14.48|||29.18|||20.80|1.00|25.66||36.75|22.26|32.76|10.90|21.31|
|pa||||||||||||||94.18|||94.18|
|ph|||71.44||||||64.00|93.81|63.79||61.50|14.20|73.88||61.90|
|pl||69.12||||61.48|25.75|50.50|80.20|25.75|38.56|43.43|26.15|84.37|29.88||49.55|
|pt|||||||||||18.07|29.29|||||23.68|
|ru||97.64||||87.63||||13.38|87.43||88.60|53.41|78.20||80.26|
|se|73.13|64.64|62.88||74.15|84.13||83.50|71.13|0.00|78.00|100.00|43.75|100.00|36.75||67.44|
|sg|||40.22||||||65.86|87.63|65.10||53.53|34.47|86.53||61.56|
|si||14.21||||2.77|||||28.50||15.14||||12.93|
|sk||66.88||||51.48||1.00|||80.80||63.53||||57.85|
|th|||||||||||45.00||||||45.00|
|tw||||||69.56|||78.30||78.00||75.25|31.28|83.50||70.01|
|ua||||||||||||57.57|16.63||43.43||39.21|
|us||30.96|45.84|||21.07|||8.82|42.10|51.84||34.85|40.78|44.31|50.50|34.32|
|ve|||||||||100.00||||||||100.00|
|za|||||||||6.20|50.50|1.00||33.62||13.38||18.84|
|Grand Total|49.93|49.35|48.67|50.50|50.21|50.14|50.50|49.32|49.56|47.82|49.24|50.50|48.45|49.18|48.24|46.69|49.12|

## The correlation of teams placing and spirit placing

## To-do

* division acronyms should follow WFDF guidelines
* include normalized placings column and normalized spirit placing column

