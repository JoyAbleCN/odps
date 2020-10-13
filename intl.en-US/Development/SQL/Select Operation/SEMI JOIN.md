---
keyword: [SEMI JOIN, LEFT SEMI JOIN, ]
---

# SEMI JOIN

MaxCompute supports two types of SEMI JOIN operations: LEFT SEMI JOIN and LEFT ANTI JOIN. In SEMI JOIN, the right table does not appear in the result set and is only used to filter data in the left table.

## LEFT SEMI JOIN

A `LEFT SEMI JOIN` returns all rows from the left table that match a row in the right table. For example, if the `id` value of a row in `mytable1` appears in the `id` column in `mytable2`, this row is returned in the result set.

To implement the preceding logic, execute the following statement:

```
SELECT * from mytable1 a LEFT SEMI JOIN mytable2 b on a.id=b.id;
```

In this example, only rows in `mytable1` with IDs that appear in `mytable2` are returned in the result set.``````

## LEFT ANTI JOIN

A `LEFT ANTI JOIN` returns all rows from the left table that do not have a match in the right table. For example, if the `id` value of a row in `mytable1` does not appear in the `id` column in `mytable2`, this row is returned in the result set.

To implement the preceding logic, execute the following statement:

```
SELECT * from mytable1 a LEFT ANTI JOIN mytable2 b on a.id=b.id;
```

In this example, only rows in `mytable1` with IDs that do not appear in `mytable2` are returned in the result set.``````

