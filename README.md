# Combining-and-Filtering-Data-with-PostgreSQL-9

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
```

