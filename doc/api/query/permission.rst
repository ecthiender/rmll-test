Permission
==========

.. _create_insert_permission:

create_insert_permission
------------------------

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
   * - role
     - true
     - :ref:`RoleName <RoleName>`
     - Role
   * - permission
     - true
     - InsertPermission_
     - The permission definition

``InsertPermission``
&&&&&&&&&&&&&&&&&&&&

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - check
     - true
     - :ref:`BoolExp <BoolExp>`
     - This expression has to hold true for every new row that is inserted

.. _drop_insert_permission:

drop_insert_permission
----------------------

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
   * - role
     - true
     - :ref:`RoleName <RoleName>`
     - Role

.. _create_select_permission:

create_select_permission
------------------------

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
   * - role
     - true
     - :ref:`RoleName <RoleName>`
     - Role
   * - permission
     - true
     - SelectPermission_
     - The permission definition

``SelectPermission``
&&&&&&&&&&&&&&&&&&&&

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - columns
     - true
     - :ref:`PGColumn <PGColumn>` array
     - Only these columns are selectable
   * - filter
     - true
     - :ref:`BoolExp <BoolExp>`
     - Only the rows where this expression holds true are selectable
   * - override
     - false
     - JSON Map (:ref:`RelationshipName <RelationshipName>` : SelectPermission_)
     - Override the permissions of relationships. Usually, the permissions on relationships are derived from the permissions on the underlying table.

.. _drop_select_permission:

drop_select_permission
----------------------

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
   * - role
     - true
     - :ref:`RoleName <RoleName>`
     - Role

.. _create_update_permission:

create_update_permission
------------------------

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
   * - role
     - true
     - :ref:`RoleName <RoleName>`
     - Role
   * - permission
     - true
     - UpdatePermission_
     - The permission definition

``UpdatePermission``
&&&&&&&&&&&&&&&&&&&&

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - columns
     - true
     - :ref:`PGColumn <PGColumn>` array
     - Only these columns are updatable
   * - filter
     - true
     - :ref:`BoolExp <BoolExp>`
     - Only the rows where this expression holds true are deletable

.. _drop_update_permission:

drop_update_permission
----------------------

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
   * - role
     - true
     - :ref:`RoleName <RoleName>`
     - Role

.. _create_delete_permission:

create_delete_permission
------------------------

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
   * - role
     - true
     - :ref:`RoleName <RoleName>`
     - Role
   * - permission
     - true
     - DeletePermission_
     - The permission definition

``DeletePermission``
&&&&&&&&&&&&&&&&&&&&

.. list-table::
   :header-rows: 1

   * - Key
     - Required
     - Schema
     - Description
   * - filter
     - true
     - :ref:`BoolExp <BoolExp>`
     - Only the rows where this expression holds true are deletable

.. _drop_delete_permission:

drop_delete_permission
----------------------

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
   * - role
     - true
     - :ref:`RoleName <RoleName>`
     - Role
