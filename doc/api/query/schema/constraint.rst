Constraint
==========

.. _add_foreign_key_constraint:

add_foreign_key_constraint
--------------------------

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
   * - constraint
     - true
     - TableForeignKeyConstraint_
     - The constraint definition

.. _TableForeignKeyConstraint:

TableForeignKeyConstraint
&&&&&&&&&&&&&&&&&&&&&&&&&

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - name
     - false
     - :ref:`TableName <TableName>`
     - Name of the constraint
   * - ref_table
     - true
     - :ref:`TableName <TableName>`
     - The referenced table
   * - mapping
     - true
     - JSON Map (:ref:`PGColumn <PGColumn>` : :ref:`PGColumn <PGColumn>`)
     - A mapping of columns from source table to referenced table
   * - on_update
     - false
     - :ref:`ForeignKeyConstraintPolicy <ForeignKeyConstraintPolicy>`
     -
   * - on_delete
     - false
     - :ref:`ForeignKeyConstraintPolicy <ForeignKeyConstraintPolicy>`
     -

.. _add_unique_constraint:

add_unique_constraint
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
   * - constraint
     - true
     - UniqueConstraint_
     - The constraint definition

.. _UniqueConstraint:

UniqueConstraint
&&&&&&&&&&&&&&&&

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - name
     - false
     - :ref:`ConstraintName <ConstraintName>`
     - Name of the constraint
   * - columns
     - true
     - :ref:`PGColumn <PGColumn>` array
     - The columns whose values when taken together are unique across all the rows in the table

.. _add_check_constraint:

add_check_constraint
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
   * - constraint
     - true
     - CheckConstraint_
     - The constraint definition

.. _CheckConstraint:

CheckConstraint
&&&&&&&&&&&&&&&

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - name
     - false
     - :ref:`ConstraintName <ConstraintName>`
     - Name of the constraint
   * - check
     - true
     - :ref:`BoolExp <BoolExp>`
     - The boolean expression that needs to hold true for every row in the table

.. note:: You can only use the columns of the table and not the relationships in ``check``

.. _rename_constraint:

rename_constraint
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
   * - constraint
     - true
     - :ref:`ConstraintName <ConstraintName>`
     - Name of the constraint
   * - new_name
     - true
     - :ref:`ConstraintName <ConstraintName>`
     - New name of the constraint

.. _drop_constraint:

drop_constraint
---------------

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
   * - constraint
     - true
     - :ref:`ConstraintName <ConstraintName>`
     - Name of the constraint
   * - cascade
     - false
     - Boolean
     - When set to ``true``, all the dependent items on this table are also dropped

.. note:: Be careful when using ``cascade``. First, try running the query without ``cascade`` or ``cascade`` set to ``false``.
