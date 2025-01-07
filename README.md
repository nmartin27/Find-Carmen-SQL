# Find-Carmen-SQL

-- Clue #1: We recently got word that someone fitting Carmen Sandiego's description has been traveling through Southern Europe. She's most likely traveling someplace where she won't be noticed, so find the least populated country in Southern Europe, and we'll start looking for her there.
        SELECT * FROM COUNTRY WHERE population < 2000 AND region = 'Southern Europe';
 VAT  | Holy See (Vatican City State) | Europe    | Southern Europe |         0.4 |      1929 |       1000 

-- Clue #2: Now that we're here, we have insight that Carmen was seen attending language classes in this country's officially recognized language. Check our databases and find out what language is spoken in this country, so we can call in a translator to work with you.
carmen=# SELECT * FROM countrylanguage WHERE countrycode = 'VAT';
 countrycode | language | isofficial | percentage
-------------+----------+------------+------------
 VAT         | Italian  | t          |          0
(1 row)


-- Clue #3: We have new news on the classes Carmen attended – our gumshoes tell us she's moved on to a different country, a country where people speak only the language she was learning. Find out which nearby country speaks nothing but that language.
carmen=# SELECT * FROM countrylanguage WHERE language = 'Italian';
 countrycode | language | isofficial | percentage
-------------+----------+------------+------------
 ITA         | Italian  | t          |       94.1
 SMR         | Italian  | t          |        100 **** San Marino


-- Clue #4: We're booking the first flight out – maybe we've actually got a chance to catch her this time. There are only two cities she could be flying to in the country. One is named the same as the country – that would be too obvious. We're following our gut on this one; find out what other city in that country she might be flying to.
carmen=# SELECT * FROM city WHERE countrycode = 'SMR';
  id  |    name    | countrycode |     district      | population
------+------------+-------------+-------------------+------------
 3170 | Serravalle | SMR         | Serravalle/Dogano |       4802
 3171 | San Marino | SMR         | San Marino        |       2294
(2 rows)


-- Clue #5: Oh no, she pulled a switch – there are two cities with very similar names, but in totally different parts of the globe! She's headed to South America as we speak; go find a city whose name is like the one we were headed to, but doesn't end the same. Find out the city, and do another search for what country it's in. Hurry!
carmen=# SELECT * FROM city WHERE name LIKE '%Ser%';
  id  |         name         | countrycode |     district      | population
------+----------------------+-------------+-------------------+------------
  265 |*** Serra                | BRA         | Espï¿½rito Santo  |     302666
  310 | Taboï¿½o da Serra    | BRA         | Sï¿½o Paulo       |     197550
  370 | Itapecerica da Serra | BRA         | Sï¿½o Paulo       |     126672
  428 | Sertï¿½ozinho        | BRA         | Sï¿½o Paulo       |      98140
  538 | Bandar Seri Begawan  | BRN         | Brunei and Muara  |      21484
  572 | La Serena            | CHL         | Coquimbo          |     137409
  903 | Serekunda            | GMB         | Kombo St Mary     |     102600
  994 | Serang               | IDN         | West Java         |     122400
 1229 | Serampore            | IND         | West Bengali      |     137028
 2474 | Seremban             | MYS         | Negeri Sembilan   |     182869
 3170 | Serravalle           | SMR         | Serravalle/Dogano |       4802
 3708 | Serpuhov             | RUS         | Moskova           |     132000
 3726 | Sergijev Posad       | RUS         | Moskova           |     111100
 3743 | Serov                | RUS         | Sverdlovsk        |     100400
(14 rows)

-- Clue #6: We're close! Our South American agent says she just got a taxi at the airport, and is headed towards the capital! Look up the country's capital, and get there pronto! Send us the name of where you're headed and we'll follow right behind you!
carmen=# SELECT * FROM country WHERE code = 'BRA';
 capital | code2
------+-----------+-----------+------------------+---------------------------+---------+-------
39.00 | 804108.00 | Brasil    | Federal Republic | Fernando Henrique Cardoso |     211 Capital?

-- Clue #7: She knows we're on to her – her taxi dropped her off at the international airport, and she beat us to the boarding gates. We have one chance to catch her, we just have to know where she's heading and beat her to the landing dock.
carmen=# SELECT * FROM city WHERE population = 91084;
  id  |     name     | countrycode |  district  | population
------+--------------+-------------+------------+------------
 4060 | Santa Monica | USA         | California |      91084
(1 row)
