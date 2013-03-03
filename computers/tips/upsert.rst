Super portable SQL upsert
==========================

While working on a SQL script that I want to work no matter what the server type is, I find this to be the best upsert method

Suppose there is a table, 'double_table', that holds double precision data, 'data', and a miscellaneous message, 'misc'. The following will either update or insert depending on whether or not the unique data value (3.2) exists:


    update double_table set misc = 'First update' where data = 3.2;
    insert into double_table(data,misc) select 3.2,'First insert' from (select 1 as value) as T where not exists (select 1 from double_table where double_table.data = 3.2);
