Relationship
============

.. _create_object_relationship:

create_object_relationship
--------------------------

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
   * - name
     - true
     - :ref:`RelationshipName <RelationshipName>`
     - Name of the new relationship
   * - using
     - true
     - :ref:`PGColumn <PGColumn>`
     - The column with foreign key constraint

.. _create_array_relationship:

create_array_relationship
-------------------------

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
   * - name
     - true
     - :ref:`RelationshipName <RelationshipName>`
     - Name of the new relationship
   * - using
     - true
     - ArrRelUsing_
     -

``ArrRelUsing``
&&&&&&&&&&&&&&&

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
     - Name of the column with foreign key constraint

.. _drop_relationship:

drop_relationship
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
   * - name
     - true
     - :ref:`RelationshipName <RelationshipName>`
     - Name of the relationship that needs to be dropped
   * - cascade
     - false
     - Boolean
     - When set to ``true``, all the dependent items on this relationship are also dropped

.. note:: Be careful when using ``cascade``. First, try running the query without ``cascade`` or ``cascade`` set to ``false``.
