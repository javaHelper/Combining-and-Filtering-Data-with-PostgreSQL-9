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
```

