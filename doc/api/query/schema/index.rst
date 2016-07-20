Index
=====

CreateIndex
-----------

Syntax
^^^^^^

.. parsed-literal::
   :class: haskell-pre

   {
      "table"   : :ref:`TableName <TableName>`,
      "columns" : [
           {
           "column" : :ref:`PGColumn <PGColumn>`,
           "order"  ? IndexSortOrder_,
           "nulls"  ? IndexNullsOrder_
           }
      ]
      "name"    ?  String,
      "unique"  ? Boolean,
      "using"   ? IndexMethod_,
      "with"    ? IndexParams_,
      "where"   ? :ref:`BoolExp <BoolExp>`
   }

``IndexSortOrder``
^^^^^^^^^^^^^^^^^^

.. parsed-literal::
   :class: haskell-pre

      "asc" | "desc"

``IndexNullsOrder``
^^^^^^^^^^^^^^^^^^^

.. parsed-literal::
   :class: haskell-pre

      "first" | "last"

If not specified, this defaults to ``first`` for IndexSortOrder_ ``desc`` and ``last`` for IndexSortOrder_ ``asc``.

``IndexMethod``
^^^^^^^^^^^^^^^

.. parsed-literal::
   :class: haskell-pre

      "btree" | "hash" | "gin" | "gist" | "spgist" | "brin"

This defaults to ``btree``.

``IndexParams``
^^^^^^^^^^^^^^^

.. parsed-literal::
   :class: haskell-pre

   {
      "fillfactor"             ? Integer,
      "fastupdate"             ? Boolean,
      "buffering"              ? Boolean,
      "gin_pending_list_limit" ? Kilobytes,
      "pages_per_range"        ? Integer
   }

+----------------------------------------+----------------------------------------+
|IndexMethod_                            |Allowed Parameters                      |
+========================================+========================================+
|gist                                    |buffering, fillfactor                   |
+----------------------------------------+----------------------------------------+
|gin                                     |fastupdate, gin_pending_list_limit      |
+----------------------------------------+----------------------------------------+
|brin                                    |pages_per_range                         |
+----------------------------------------+----------------------------------------+
|btree                                   |pages_per_range, fillfactor             |
+----------------------------------------+----------------------------------------+
|hash                                    |pages_per_range, fillfactor             |
+----------------------------------------+----------------------------------------+
|spgist                                  |pages_per_range, fillfactor             |
+----------------------------------------+----------------------------------------+

DropIndex
---------

Syntax
^^^^^^

.. parsed-literal::
   :class: haskell-pre

   {
      "index"   : String
   }
