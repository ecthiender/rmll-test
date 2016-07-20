HTTP API
========

Introspect
----------

You can fetch the database schema as well as all the defined relationships and permissions.

Request
^^^^^^^

  .. code-block:: http

     GET /v1/table HTTP/1.1

Response
^^^^^^^^

  .. parsed-literal::
     :class: haskell-pre

     [TableMetaData]

Query
-----
HasuraDB unifies all operations that can be performed on the database under a single query interface.

HasuraDB exposes a query endpoint at ``v1/query``. A typical query operation is as follows:

Request
^^^^^^^

  .. code-block:: http

     POST /v1/query HTTP/1.1

  Takes the JSON structure of a :ref:`Query <query_def>` for its body

.. _query_def:

``Query``
&&&&&&&&&

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - type
     - true
     - String
     - Type of the query
   * - args
     - true
     - JSON Value
     - The arguments to the query

The various values that ``type`` and ``args`` can take are listed in the following table:

  .. list-table::
     :header-rows: 1

     * - ``type``
       - ``args``

     * - ``"insert"``
       - :ref:`insert <data_insert>`
     * - ``"select"``
       - :ref:`select <data_select>`
     * - ``"update"``
       - :ref:`update <data_update>`
     * - ``"delete"``
       - :ref:`delete <data_delete>`
     * - ``"count"``
       - :ref:`count <data_count>`

     * - ``"create_table"``
       - :ref:`create_table <create_table>`
     * - ``"add_existing_table"``
       - :ref:`add_existing_table <add_existing_table>`
     * - ``"drop_table"``
       - :ref:`drop_table <drop_table>`
     * - ``"add_column"``
       - :ref:`add_column <add_column>`
     * - ``"drop_column"``
       - :ref:`drop_column <drop_column>`
     * - ``"alter_column_type"``
       - :ref:`alter_column_type <alter_column_type>`
     * - ``"alter_column_default"``
       - :ref:`alter_column_default <alter_column_default>`
     * - ``"alter_column_nullable"``
       - :ref:`alter_column_nullable <alter_column_nullable>`

     * - ``"add_foreign_key_constraint"``
       - :ref:`add_foreign_key_constraint <add_foreign_key_constraint>`
     * - ``"add_unique_constraint"``
       - :ref:`add_unique_constraint <add_unique_constraint>`
     * - ``"add_check_constraint"``
       - :ref:`add_check_constraint <add_check_constraint>`
     * - ``"rename_constraint"``
       - :ref:`rename_constraint <rename_constraint>`
     * - ``"drop_constraint"``
       - :ref:`drop_constraint <drop_constraint>`

     * - ``"create_object_relationship"``
       - :ref:`create_object_relationship <create_object_relationship>`
     * - ``"create_array_relationship"``
       - :ref:`create_array_relationship <create_array_relationship>`
     * - ``"drop_relationship"``
       - :ref:`drop_relationship <drop_relationship>`

     * - ``"create_insert_permission"``
       - :ref:`create_insert_permission <create_insert_permission>`
     * - ``"drop_insert_permission"``
       - :ref:`drop_insert_permission <drop_insert_permission>`

     * - ``"create_select_permission"``
       - :ref:`create_select_permission <create_select_permission>`
     * - ``"drop_select_permission"``
       - :ref:`drop_select_permission <drop_select_permission>`

     * - ``"create_update_permission"``
       - :ref:`create_update_permission <create_update_permission>`
     * - ``"drop_update_permission"``
       - :ref:`drop_update_permission <drop_update_permission>`

     * - ``"create_delete_permission"``
       - :ref:`create_delete_permission <create_delete_permission>`
     * - ``"drop_delete_permission"``
       - :ref:`drop_delete_permission <drop_delete_permission>`

     * - ``"bulk"``
       - :ref:`Query <query_def>` array

Response
^^^^^^^^

The response structure is dependent on the type of query that is executed.

Errors
------

.. list-table::
   :widths: 10 10 30
   :header-rows: 1

   * - Status code
     - Description
     - Response structure

   * - ``200``
     - Success
     - .. parsed-literal::

          Request specific

   * - ``400``
     - Bad request
     - .. code-block:: haskell

          {
              "path"  : String,
              "error" : String
          }

   * - ``401``
     - Unauthorized
     - .. code-block:: haskell

          {
              "error" : String
          }

   * - ``500``
     - Internal server error
     - .. code-block:: haskell

          {
              "error" : String
          }
