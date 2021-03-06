---
title: pgroonga.flush function
upper_level: ../
---

# `pgroonga.flush` function

## Summary

`pgroonga.flush` function ensuring writing changes only in memory into disk. Normally, you don't need to this function because it's done automatically. But you may need to use this function when you want to prevent breaking PGroonga indexes on crash or force shutdown.

Normally, users shouldn't shut down server forcibly but some users do in some cases. For example, Windows update may restart Windows server unexpectedly.

If PostgreSQL with PGroonga is shut down forcibly, changes only in memory may be lost. If you call `pgroonga.flush` function before force shutdown, there are no changes only in memory. It means that PGroonga indexes aren't broken even if users shut down PostgreSQL with PGroonga forcibly.

If there are many changes only in memory, `pgroonga.flush` may take a long time. It's depend on write performance of your disk.

## Syntax

Here is the syntax of this function:

```text
bool pgroonga.flush(pgroonga_index_name)
```

`pgroonga_index_name` is a `text` type value. It's an index name to be flushed. The index should be created with `USING pgroonga`.

`pgroonga.flush` returns always `true`. Because if `pgroonga.flush` is failed, it raises an error instead of returning result.

## Usage

Here are sample schema and data. In the schema, both search target data and output data are index target columns:

```sql
CREATE TABLE terms (
  id integer,
  title text,
  content text
);

CREATE INDEX pgroonga_terms_index
          ON terms
       USING pgroonga (title, content);
```

You can flush `pgroonga_terms_index` related changes only in memory by the following `pgroonga.flush` call:

```sql
SELECT pgroonga.flush('pgroonga_terms_index');
--  flush 
-- -------
--  t
-- (1 row)
```

If you specify nonexistent index name, `pgroonga.flush` raises an error:

```sql
SELECT pgroonga.flush('nonexistent');
-- ERROR:  relation "nonexistent" does not exist
```
