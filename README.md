# Combining-and-Filtering-Data-with-PostgreSQL-9

# Working with String functions

```sql
select trim(' radar '); 

`radar`

select trim('r' from 'radar'); 

`ada`

select trim(leading 'r' from 'radar'); 

`adar`

select trim(trailing 'r' from 'radar');  

`rada`


select split_part('USA/DC/202', '/', 2); 

`DC`

select substring('USA/DC/202', 5, 2); 

`DC`

select substring('USA/DC/202' from 5 for 2); 

`DC`


select substring('USA/DC/202',5);

`DC/202`


select carrier_code || ': ' || carrier_desc 
from codes_carrier cc ;
?column?              |
----------------------+
AA: American Airlines |
AS: Alaska Airlines   |
B6: JetBlue Airways   |
DL: Delta Air Lines   |
F9: Frontier Airlines |
G4: Allegiant Air     |
HA: Hawaiian Airlines |
NK: Spirit Air Lines  |
UA: United Air Lines  |
VX: Virgin America    |
WN: Southwest Airlines|


select concat(carrier_code ,': ', carrier_desc) as display_carrier 
from codes_carrier cc ;
display_carrier       |
----------------------+
AA: American Airlines |
AS: Alaska Airlines   |
B6: JetBlue Airways   |
DL: Delta Air Lines   |
F9: Frontier Airlines |
G4: Allegiant Air     |
HA: Hawaiian Airlines |
NK: Spirit Air Lines  |
UA: United Air Lines  |
VX: Virgin America    |
WN: Southwest Airlines|

select carrier_code ,
		carrier_desc ,
		substring(carrier_desc, 1, 8) as carrier_display 
from codes_carrier cc ;
carrier_code|carrier_desc      |carrier_display|
------------+------------------+---------------+
AA          |American Airlines |American       |
AS          |Alaska Airlines   |Alaska A       |
B6          |JetBlue Airways   |JetBlue        |
DL          |Delta Air Lines   |Delta Ai       |
F9          |Frontier Airlines |Frontier       |
G4          |Allegiant Air     |Allegian       |
HA          |Hawaiian Airlines |Hawaiian       |
NK          |Spirit Air Lines  |Spirit A       |
UA          |United Air Lines  |United A       |
VX          |Virgin America    |Virgin A       |
WN          |Southwest Airlines|Southwes       |


select carrier_code ,
		carrier_desc ,
		upper(replace (substring(carrier_desc, 1, 8), ' ','')) as carrier_display 
from codes_carrier cc ;

carrier_code|carrier_desc      |carrier_display|
------------+------------------+---------------+
AA          |American Airlines |AMERICAN       |
AS          |Alaska Airlines   |ALASKAA        |
B6          |JetBlue Airways   |JETBLUE        |
DL          |Delta Air Lines   |DELTAAI        |
F9          |Frontier Airlines |FRONTIER       |
G4          |Allegiant Air     |ALLEGIAN       |
HA          |Hawaiian Airlines |HAWAIIAN       |
NK          |Spirit Air Lines  |SPIRITA        |
UA          |United Air Lines  |UNITEDA        |
VX          |Virgin America    |VIRGINA        |
WN          |Southwest Airlines|SOUTHWES       |

```
--------

# Aggregating Function

```sql
select 2+2;

select 12/2;

select 13/2;

select 13/2::float;

select 15%2;

select 12 ^ 2;

select |/36;

select @ (36-40);

select abs(36-40); 
```
----

# Aggregate Functions in Action

```sql
select avg(dep_delay_new) 
from performance p  ;

select 	mkt_carrier ,
		avg(dep_delay_new) 
from performance p  
group by mkt_carrier ;
mkt_carrier|avg                |
-----------+-------------------+
AA         |12.6209875916668781|
AS         | 6.0100193355598523|
B6         |24.1109308283518360|
DL         |15.6977092251380359|
F9         |19.6507352941176471|
G4         |14.5763313609467456|
HA         | 5.3746002132196162|
NK         |10.3558917197452229|
UA         |14.7194754109927327|
VX         | 8.1761621810555750|
WN         | 9.9191719507646401|


select 	p.mkt_carrier ,
		avg(p.dep_delay_new) 
from performance p  
inner join codes_carrier cc 
	on p.mkt_carrier  = cc.carrier_code 
group by p.mkt_carrier ;
mkt_carrier|avg                |
-----------+-------------------+
AA         |12.6209875916668781|
AS         | 6.0100193355598523|
B6         |24.1109308283518360|
DL         |15.6977092251380359|
F9         |19.6507352941176471|
G4         |14.5763313609467456|
HA         | 5.3746002132196162|
NK         |10.3558917197452229|
UA         |14.7194754109927327|
VX         | 8.1761621810555750|
WN         | 9.9191719507646401|

select 	p.mkt_carrier ,
		avg(p.dep_delay_new) 
from performance p  
inner join codes_carrier cc 
	on p.mkt_carrier  = cc.carrier_code 
group by p.mkt_carrier, cc.carrier_desc  ;
mkt_carrier|avg                |
-----------+-------------------+
AA         |12.6209875916668781|
AS         | 6.0100193355598523|
B6         |24.1109308283518360|
DL         |15.6977092251380359|
F9         |19.6507352941176471|
G4         |14.5763313609467456|
HA         | 5.3746002132196162|
NK         |10.3558917197452229|
UA         |14.7194754109927327|
VX         | 8.1761621810555750|
WN         | 9.9191719507646401|


select 	p.mkt_carrier ,
		avg(p.dep_delay_new) 
from performance p  
inner join codes_carrier cc 
	on p.mkt_carrier  = cc.carrier_code 
group by p.mkt_carrier, 
		cc.carrier_desc  
order by avg(p.dep_delay_new) ;
mkt_carrier|avg                |
-----------+-------------------+
HA         | 5.3746002132196162|
AS         | 6.0100193355598523|
VX         | 8.1761621810555750|
WN         | 9.9191719507646401|
NK         |10.3558917197452229|
AA         |12.6209875916668781|
G4         |14.5763313609467456|
UA         |14.7194754109927327|
DL         |15.6977092251380359|
F9         |19.6507352941176471|
B6         |24.1109308283518360|


select 	p.mkt_carrier ,
		cc.carrier_desc ,
		avg(p.dep_delay_new) 
from performance p  
inner join codes_carrier cc 
	on p.mkt_carrier  = cc.carrier_code 
group by p.mkt_carrier, 
		cc.carrier_desc  
order by avg(p.dep_delay_new) ;
mkt_carrier|carrier_desc      |avg                |
-----------+------------------+-------------------+
HA         |Hawaiian Airlines | 5.3746002132196162|
AS         |Alaska Airlines   | 6.0100193355598523|
VX         |Virgin America    | 8.1761621810555750|
WN         |Southwest Airlines| 9.9191719507646401|
NK         |Spirit Air Lines  |10.3558917197452229|
AA         |American Airlines |12.6209875916668781|
G4         |Allegiant Air     |14.5763313609467456|
UA         |United Air Lines  |14.7194754109927327|
DL         |Delta Air Lines   |15.6977092251380359|
F9         |Frontier Airlines |19.6507352941176471|
B6         |JetBlue Airways   |24.1109308283518360|


select 	p.mkt_carrier ,
		cc.carrier_desc ,
		avg(p.dep_delay_new) as departure_delay,
		avg(p.arr_delay_new) as arrival_delay 
from performance p  
inner join codes_carrier cc 
	on p.mkt_carrier  = cc.carrier_code 
group by p.mkt_carrier, 
		cc.carrier_desc  
order by 2 ;
mkt_carrier|carrier_desc      |departure_delay    |arrival_delay      |
-----------+------------------+-------------------+-------------------+
AS         |Alaska Airlines   | 6.0100193355598523| 6.5396359780880014|
G4         |Allegiant Air     |14.5763313609467456|14.7267459138187221|
AA         |American Airlines |12.6209875916668781|13.1136531412279809|
DL         |Delta Air Lines   |15.6977092251380359|14.9632170882588076|
F9         |Frontier Airlines |19.6507352941176471|18.7577574418849269|
HA         |Hawaiian Airlines | 5.3746002132196162| 6.2211358156978302|
B6         |JetBlue Airways   |24.1109308283518360|23.7532361765966567|
WN         |Southwest Airlines| 9.9191719507646401| 8.4052680264989769|
NK         |Spirit Air Lines  |10.3558917197452229|10.6143198782520472|
UA         |United Air Lines  |14.7194754109927327|15.0360204921296419|
VX         |Virgin America    | 8.1761621810555750| 8.9830271216097988|

```
------

# Filtering Aggregate Functions

```sql
select 	p.mkt_carrier ,
		cc.carrier_desc ,
		avg(p.dep_delay_new) as departure_delay,
		avg(p.arr_delay_new) as arrival_delay 
from performance p  
inner join codes_carrier cc 
	on p.mkt_carrier  = cc.carrier_code 
group by p.mkt_carrier, 
		cc.carrier_desc  
having  avg(p.dep_delay_new) > 15
		and avg(p.arr_delay_new) > 15;
		
mkt_carrier|carrier_desc     |departure_delay    |arrival_delay      |
-----------+-----------------+-------------------+-------------------+
B6         |JetBlue Airways  |24.1109308283518360|23.7532361765966567|
F9         |Frontier Airlines|19.6507352941176471|18.7577574418849269|		
```
-------

# Joins

```sql
select 	p.fl_date ,
		p.mkt_carrier ,
		cc.carrier_desc as airline,
		p.mkt_carrier_fl_num as flight,
		p.origin_city_name ,
		p.dest_city_name ,
		p.cancellation_code ,
		ca.cancel_desc 
from performance p  
inner join codes_carrier cc 
	on p.mkt_carrier  = cc.carrier_code 
left  join codes_cancellation ca
	on p.cancellation_code = ca.cancellation_code ;
```

------

# Set Theory

```sql
select 	*
from performance p  
where origin = 'BIL'
	and dest = 'SEA'
	and fl_date = '2018-01-01';
fl_date   |mkt_carrier|mkt_carrier_fl_num|origin|origin_city_name|origin_state_abr|dest|dest_city_name|dest_state_abr|dep_delay_new|arr_delay_new|cancelled|cancellation_code|diverted|carrier_delay|weather_delay|nas_delay|security_delay|late_aircraft_delay|
----------+-----------+------------------+------+----------------+----------------+----+--------------+--------------+-------------+-------------+---------+-----------------+--------+-------------+-------------+---------+--------------+-------------------+
2018-01-01|AS         |2423              |BIL   |Billings, MT    |MT              |SEA |Seattle, WA   |WA            |           22|            0|        0|                 |       0|             |             |         |              |                   |
2018-01-01|AS         |2433              |BIL   |Billings, MT    |MT              |SEA |Seattle, WA   |WA            |            2|            0|        0|                 |       0|             |             |         |              |                   |
```
