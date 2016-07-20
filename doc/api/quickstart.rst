Quickstart
==========

.. _Object: http://rfc7159.net/rfc7159#rfc.section.4

In this quickstart, we'll show you how to use HasuraDB for a minimal blogging application.

Modelling data
--------------

As you would normally do with any relational database, data is modelled as tables. Once you have the tables, you can define relationships and permissions.

Assume we have the following tables

+----------------------------------------+----------------------------------------+
|Table                                   |Columns                                 |
+========================================+========================================+
|author                                  |id, name                                |
+----------------------------------------+----------------------------------------+
|article                                 |id, title, author_id, rating            |
+----------------------------------------+----------------------------------------+

Let's create these tables.

author
######

.. code-block:: http

   POST /v1/query HTTP/1.1
   Content-Type: application/json
   Authorization: <admin-token>

   {
       "type": "create_table",
       "args": {
           "name": "author",
           "primary_key": ["id"],
           "columns": [
               { "name": "id", "type": "integer" },
               { "name": "name", "type": "text" }
           ]
       }
   }

article
#######

.. code-block:: http

   POST /v1/query HTTP/1.1
   Content-Type: application/json
   Authorization: <admin-token>

   {
       "type": "create_table",
       "args": {
           "name": "article",
           "primary_key": ["id"],
           "columns": [
               { "name": "id", "type": "serial" },
               { "name": "title", "type": "text" },
               { "name": "rating", "type": "numeric" },
               { "name": "content", "type": "text" },
               { "name": "author_id", "type": "integer" }
           ]
       }
   }

Now let's add a foreign key constraint on the ``author_id`` column. It references ``id`` column of ``author`` table.

.. code-block:: http

   POST /v1/query HTTP/1.1
   Content-Type: application/json
   Authorization: <admin-token>

   {
       "type": "add_foreign_key_constraint",
       "args": {
           "table": "article",
           "constraint": {
               "ref_table" : "author",
               "mapping" : { "author_id" : "id" }
           }
       }
   }

Using this foreign key constraint, we can define the following relationships:

1. article's author
2. author's list of articles

.. code-block:: http

   POST /v1/query HTTP/1.1
   Content-Type: application/json
   Authorization: <admin-token>

   {
       "type": "bulk",
       "args": [
           {
               "type": "create_object_relationship",
               "args": {
                   "table": "article",
                   "name": "author",
                   "using": "author_id"
               }
           },
           {
               "type": "create_array_relationship",
               "args": {
                   "table": "author",
                   "name": "articles",
                   "using": {
                       "table": "article",
                       "column": "author_id"
                   }
               }
           }
       ]
   }

Inserting Data
--------------

Inserting data into tables is straightforward. You can insert data only into non-relationship column. The full definition of `insert` request can be found :ref:`here <data_insert>`.

.. code-block:: http

   POST /v1/query HTTP/1.1
   Content-Type: application/json

   {
     "type" : "insert",
     "args" : {
       "returning": ["id"],
       "objects": [
         {"id" : 1, "name": "Dejesus"},
         {"id" : 2, "name": "Mccarthy"}
       ]
   }

Querying Data
-------------

Querying data is much more complex, as relationship columns can be involved in interesting ways.

Simple Queries
##############

Let's look at a simple `select` query on the **author** table. The full definition of a `select` query can be found :ref:`here <data_select>`

.. code-block:: http

   POST /v1/query HTTP/1.1
   Content-Type: application/json

   {
     "type" : "select",
     "args" : {
       "table" : "author",
       "columns": ["id"],
       "where": {"name": "John"}
     }
   }

This query returns ids of all authors named 'John'.

Boolean operators like ``$and``, ``$or`` can be used in a `where` clause. See :ref:`here <BoolExp>` for a full list of supported Boolean operators.

.. code-block:: http

   POST /v1/query HTTP/1.1
   Content-Type: application/json

   {
     "type" : "select",
     "args" : {
       "table" : "author",
       "columns": ["id"],
       "where": {
         "$or": [
           {"name": "John"},
           {"name": "Johnny"}
         ]}
     }
   }

.. note::

   .. code-block:: json

      { "name": "John" }

   is just a shortcut for writing the 'is-equal-to' operator, ``$eq``

   .. code-block:: json

     { "name": { "$eq": "John" } }

Order-By, Limit and Offset
$$$$$$$$$$$$$$$$$$$$$$$$$$

``order_by`` is used to sort the results by a column. A prefix of ``+`` or ``-`` indicates ascending or descending order respectively.

``limit`` and ``offset`` are used to slice the result set.

Example,

.. code-block:: http

   POST /v1/query HTTP/1.1
   Content-Type: application/json

   {
     "type" : "select",
     "args" : {
        "table" : "author",
        "columns": ["name"],
        "order_by": "+id",
        "limit": 10,
        "offset": 2
      }
   }

.. note:: Ordering by **id** column is allowed, even though it is not present in the result.


Extracting Information from Relationships
#########################################

Next, we see how to extract information from relationship columns. To obtain the **author**'s name from the article table, we issue,

.. code-block:: http

   POST /v1/query HTTP/1.1
   Content-Type: application/json

   {
     "type" : "select",
     "args" : {
        "table" : "article",
        "columns": [
          "title",
          {"name": "author", "columns": ["name"]}
        ]
      }
   }

The same syntax can be used to obtain the titles of all articles across all **authors**.

.. code-block:: http

   POST /v1/query HTTP/1.1
   Content-Type: application/json

   {
     "type" : "select",
     "args" : {
         "table" : "author",
         "columns": [
           "name",
           {"name": "articles", "columns": ["title"]}
         ]
       }
   }

As you probably guessed, ``columns`` can be nested to include nested relationships.

Filtering on relationship columns
#################################

The most powerful feature of HasuraDB is the ability to filter on data in relationship columns.

Fetch the titles of all articles by "John"
$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$

Here we see how to query based on an `object-relationship`.

.. code-block:: http

   POST /v1/query HTTP/1.1
   Content-Type: application/json

   {
     "type" : "select",
     "args" : {
       "table" : "article",
       "columns" : ["title"],
       "where": {
         "author": {"name": "John"}
       }
     }
   }

Fetch all authors having at least one highly rated article
$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$

Here we see how to query based on an `array relationship`.

.. code-block:: http

   POST /v1/query HTTP/1.1
   Content-Type: application/json

   {
     "type" : "select",
     "args" : {
       "table" : "author",
       "columns" : ["name"],
       "where": {
         "articles": {"rating": {"$gte": 4.5}}
       }
     }
   }

This query is read as "fetch the name of all authors whose list of articles contains at least one with rating >= 4.5".

Fetch top 2 articles for each author
$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$

We can use `order_by` and `limit` in array relationships to issue more interesting queries.

.. code-block:: http

   POST /v1/query HTTP/1.1
   Content-Type: application/json

   {
     "type" : "select",
     "args" : {
       "table" : "author",
       "columns" : [
         "name",
         {
           "name": "articles",
           "columns": ["title"],
           "order_by": "+rating",
           "limit" : 2
         }
       ]
     }
   }


Updating Data
-------------

The request to update data consists of two parts - the new values and a where-clause indicating what to update. The syntax of where clause is exactly same as the `select` query. For the full syntax of update request, see :ref:`here <data_update>`.

.. code-block:: http

   POST /v1/query HTTP/1.1
   Content-Type: application/json

   {
     "type" : "update",
     "args" : {
       "table" : "article",
       "$set": {"title": "Mysterious affair at Styles"},
       "where": {
           "id": 4
       }
     }
   }

Delete Data
-----------

The request to delete data takes a where-clause indicating what to delete. The syntax of where clause is exactly same as the `select` query. For the full syntax of delete request, see :ref:`here <data_delete>`.

.. code-block:: http

   POST /v1/query HTTP/1.1
   Content-Type: application/json

   {
     "type" : "delete",
     "args" : {
       "table" : "article",
       "where": {
          "rating": {"$lte" : 1}
       }
     }
   }
