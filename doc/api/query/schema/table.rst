Table
=====

.. _add_existing_table:

add_existing_table
------------------

Tracks an existing table. HasuraDB, by default will not track the tables that are not created through HasuraDB. Once a table is tracked, you can define relationships and permissions on the table.

Syntax
^^^^^^

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - table
     - true
     - :ref:`TableName <TableName>`
     - Name of the table

.. _create_table:

create_table
------------

Creates a new table and tracks it.

Syntax
^^^^^^

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - name
     - true
     - :ref:`TableName <TableName>`
     - Name of the table
   * - columns
     - true
     - :ref:`ColumnDeclaration <ColumnDeclaration>` array
     - Columns
   * - primary_key
     - true
     - :ref:`PGColumn <PGColumn>` array
     - Primary key. If you specify more than one, it is a composite primary key

.. _ColumnDeclaration:

``ColumnDeclaration``
&&&&&&&&&&&&&&&&&&&&&

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - name
     - true
     - :ref:`PGColumn <PGColumn>`
     - Name of the column
   * - type
     - true
     - :ref:`PGColumnType <PGColumnType>`
     - The type of the column
   * - nullable
     - false
     - Boolean
     - Whether a ``null`` can be set for this column
   * - unique
     - false
     - Boolean
     - Whether the values are unique across all rows for this column
   * - default
     - false
     - ColumnDefault_
     - The default value for this column
   * - references
     - false
     - ColumnForeignKeyConstraint_
     - Does this column reference any other table's column? Essentially adding a foreign key constraint.

``ColumnDefault``
&&&&&&&&&&&&&&&&&

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - value
     - false
     - Value
     - An appropriate value based on the type of the column
   * - expression
     - false
     - String
     - An SQL expression

.. note:: Only one of ``value`` and ``expression`` should be present, and both cannot be absent.

``ColumnForeignKeyConstraint``
&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - ref_table
     - true
     - :ref:`TableName <TableName>`
     - The table which is being referred to
   * - ref_column
     - true
     - :ref:`PGColumn <PGColumn>`
     - The column of the referred table
   * - on_update
     - false
     - :ref:`ForeignKeyConstraintPolicy <ForeignKeyConstraintPolicy>`
     -
   * - on_delete
     - false
     - :ref:`ForeignKeyConstraintPolicy <ForeignKeyConstraintPolicy>`
     -

.. _ForeignKeyConstraintPolicy:

``ForeignKeyConstraintPolicy``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A String. Allowed values are ``no_action``, ``restrict``, ``cascade``, ``set_null``, ``set_default``

.. _drop_table:

drop_table
----------

Drops an existing table. If ``cascade`` is ``true``, all the dependent tables are also dropped. Refer to Postgres documentation.

Syntax
^^^^^^

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - table
     - true
     - :ref:`TableName <TableName>`
     - Name of the table that needs to be dropped
   * - cascade
     - false
     - Boolean
     - When set to ``true``, all the dependent items on this table are also dropped

.. note:: Be careful when using ``cascade``. First, try running the query without ``cascade`` or ``cascade`` set to ``false``.

.. _add_column:

add_column
----------

Syntax
^^^^^^

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - table
     - true
     - :ref:`TableName <TableName>`
     - Name of the table
   * - column
     - true
     - :ref:`ColumnDeclaration <ColumnDeclaration>`
     - The new column that has to be added

.. _drop_column:

drop_column
-----------

Syntax
^^^^^^

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - table
     - true
     - :ref:`TableName <TableName>`
     - Name of the table
   * - column
     - true
     - :ref:`PGColumn <PGColumn>`
     - Name of the column that needs to be dropped
   * - cascade
     - false
     - Boolean
     - When set to ``true``, all the dependent items on this column are also dropped

.. note:: Be careful when using ``cascade``. First, try running the query without ``cascade`` or ``cascade`` set to ``false``.

.. _alter_column_type:

alter_column_type
-----------------

Syntax
^^^^^^

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - table
     - true
     - :ref:`TableName <TableName>`
     - Name of the table
   * - column
     - true
     - :ref:`PGColumn <PGColumn>`
     - Name of the column
   * - new_type
     - true
     - :ref:`PGColumnType <PGColumnType>`
     - New type

.. note:: The existing values of the column should be compatible with the ``new_type`` that is being set. For example, you can always change the type of a column from ``integer`` to ``text``, but when changing from ``text`` to ``integer``, this operation could fail. In this case, add a new column with the new type, copy the contents of this column to the new column, converting them appropriately as per to the new type, drop the old column and then rename the new column to the old column.

.. _alter_column_default:

alter_column_default
--------------------

Syntax
^^^^^^

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - table
     - true
     - :ref:`TableName <TableName>`
     - Name of the table
   * - column
     - true
     - :ref:`PGColumn <PGColumn>`
     - Name of the column
   * - new_default
     - true
     - ColumnDefault_
     - The new default value of the column

.. note:: Only the new rows that are inserted will use this newly set default value. It does not affect the already existing rows.

.. _alter_column_nullable:

alter_column_nullable
---------------------

Syntax
^^^^^^

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - table
     - true
     - :ref:`TableName <TableName>`
     - Name of the table
   * - column
     - true
     - :ref:`PGColumn <PGColumn>`
     - Name of the column
   * - nullable
     - true
     - Boolean
     - The new nullable property
