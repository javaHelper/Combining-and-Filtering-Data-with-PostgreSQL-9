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

```

