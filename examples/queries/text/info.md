# Text Query

A family of text queries that accept text, analyzes it, and constructs a query out of it.


## Properties

* Supports exact matching (term like) on numeric values and dates.
* Does not go through a "query parsing" process (does not support field name prefixes,
  wildcard characters, or other "advance" features).
* Provides an excellent behavior when it comes to just analyze and run that text
  as a query behavior (which is usually what a text search box does).


## References

* [Text Query](http://www.elasticsearch.org/guide/reference/query-dsl/text-query.html)
