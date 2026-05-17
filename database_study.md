### Useful links

https://supabase.com/blog/postgres-full-text-search-vs-the-rest
https://about.gitlab.com/blog/2016/03/18/fast-search-using-postgresql-trigram-indexes/

https://github.com/odoo/odoo/pull/112000




Star Schema, Cubes, rollups



### Data Files

the following command to view implementation of the data file of a table

```psql
-- specific table

SELECT relname, amname FROM pg_class c
JOIN pg_am a ON c.relam = a.oid
WHERE relname = 'res_partner';

SELECT
    n.nspname AS schema_name,
    c.relname AS table_name,
    am.amname AS storage_method
FROM pg_class c
JOIN pg_namespace n ON c.relnamespace = n.oid
JOIN pg_am am ON c.relam = am.oid
WHERE
    c.relkind = 'r'  -- 'r' indicates ordinary tables
    AND n.nspname NOT IN ('pg_catalog', 'information_schema');
```

