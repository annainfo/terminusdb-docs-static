---
title: Datalog Explanation
nextjs:
  metadata:
    title: Datalog Explanation
    description: A brief explanation of Datalog and its benefits in database queries.
    openGraph:
      images: https://assets.terminusdb.com/docs/technical-documentation-terminuscms-og.png
    alternates:
      canonical: https://terminusdb.org/docs/datalog-explanation/
media: []
---

## What is Datalog?

Datalog, a declarative subset of Prolog, is a flexible and powerful declarative query language proficient at dealing with the complex and multi-hop relationships that occur in graphs. Graph query languages not based on Datalog lack the level of clarity, simplicity, and logical framework that Datalog provides.

### Predicates

Similar to its super-set Prolog, Datalog is based on **predicates**. Predicates are similar to relations in relational languages such as SQL. Queries can use predicates with _logical variables_ to represent unknowns to which meaning is assigned based on a logical formula. Meaning is assigned by joining predicates with logical connectives or operators such as `and` and `or` and [unifying](#unificationandquery) logical variables. Repeated occurrences of the same variable require the query has identical solutions at given points.

### Advantages of Datalog in queries

Variables in Datalog are restricted to finite **atomic** values. The use of atomic values simplifies query optimization and guarantees the termination of queries even in the event of recursion. The finite atomic values restriction is relaxed in [WOQL](/docs/woql-explanation/) (the Web Object Query Language used in TerminusDB) to enable lists that are useful in aggregation and dis-aggregation queries such as `group by` and `member` respectively. However, TerminusDB retains the pure declarative quality of Datalog.

#### Datalog compared with SQL

Compared with relational databases, Datalog provides a more flexible logical framework that is easier to extend consistently with recursive and path-centric operations. Datalog also enables complex joins to be expressed more elegantly with a less verbose syntax. Datalog represents a stepping-stone from relational languages such as SQL to more fully-featured programming languages while retaining the declarative, robust, pervasive, and resilient properties of query languages.

### Unification and query

Unification in Datalog is the process of finding values of logical variables which are consistent for a given logical sentence or query.

A logical variable for a query can only take on one value in a given solution. If the variable is used in two places then these two values must be the same. We can get the concrete value of solutions for a logical value either from an equation or from the definition of a predicate.

When we search using datalog in WOQL, we implicitly ask for _all_ solutions (this can be restricted by using additional words such as `limit(n,Q)`). This gives us back something that looks quite similar to a table, but it is a list of solutions with bindings for all logical variables that took on a value during the course of searching for the solutions to the query.

#### An Example

Perhaps the most important predicate in WOQL is `triple` which gives results about edges in the current graph.

Our logical variables are represented as strings with the prefix `v:`. Our edges are represented by having a position for the _subject_, _predicate_ and _object_ of the edge in the graph. The _predicate_ is the labeled name of the edge, and the _subject_ and _object_ nodes the source, and target respectively.

```datalog
triple("v:Subject", "v:Predicate", "v:Object")
```

With this query, we simply get back the solutions for every possible assignment of subjects, predicates, and objects that our graph currently has, that is, all edges in the graph. The concrete referents for the subject, predicate and object are data points represented by a URI (a universal resource indicator).

```javascript
triple("v:Subject", "v:Predicate", "v:Intermediate")
triple("v:Intermediate", "v:Predicate", "v:Object")
```

In this second query, we have _joined_ two predicates together by requiring that the target of the first edge is the source of the second. This gives us back all two-hop paths possible in the graph.

```javascript
triple("My_Object", "v:Predicate", "v:Intermediate")
triple("v:Intermediate", "v:Predicate", "v:Object")
```

And here we refer to a specific starting node and search for every two-hop path starting from _this_ object.

## Further Reading

Read more:
* the [Unification of Variables](/docs/unification-of-variables-in-datalog/)
* how to [Query with WOQL](/docs/how-to-query-with-woql/).
* [WOQL Getting Started](/docs/woql-getting-started/) guide for more examples and putting it all together!
* [JavaScript](/docs/javascript/) and [Python](/docs/python/) WOQL Reference guides

### How-to guides

See the [How-to Guides](/docs/use-the-clients/) for further examples of using WOQL in the clients.

### Documents

[Documents](/docs/documents-explanation/) in a knowledge graph and how to use them.