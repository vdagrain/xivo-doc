.. _agent-groups:

************
Agent groups
************

Agent Group Representation
==========================

Description
-----------

+-----------------------+--------+--------------------------------+
| Field                 | Values | Description                    |
+=======================+========+================================+
| id                    | int    | Read-only                      |
+-----------------------+--------+--------------------------------+
| name                  | string | Group name, lowercase no space |
+-----------------------+--------+--------------------------------+
| description           | string | Group description              |
+-----------------------+--------+--------------------------------+

Example
-------

::

    {
        "id": 45,
        "name": "homeaccounts",
        "description": "Agents managing home accounts"
    }

List Agent Groups
=================

Agent groups are listed in ascending order on name

Query
-----

::

    GET /1.1/agentgroups


Example requests
----------------

::

   GET /1.1/agentgroups HTTP/1.1
   Host: xivoserver
   Accept: application/json


Example response
----------------

::

   HTTP/1.1 200 OK
   Content-Type: application/json

   {
       "total": 2,
       "items":
       [
           {
                "id": 1,
                "name": "default",
                "description"; ""
           },
           {
                "id": 2,
                "name": "banking"
                "description": "bank calls"
           }
       ]
   }

Get Agent Group
---------------

::

    GET /1.1/agentgroups/<id>

Example request
---------------

::

   GET /1.1/agentgroups/78 HTTP/1.1
   Host: xivoserver
   Accept: application/json

Example response
----------------

::

   HTTP/1.1 200 OK
   Content-Type: application/json

   {
                "id": 78,
                "name": "insurance",
                "description": "Home Insurance"
   }

Create an Agent Group
=====================

Query
-----

::

   POST /1.1/agentgroups

Input
-----

+-----------------------+----------+--------------------------------------+
| Field                 | Required | Values                               |
+=======================+==========+======================================+
| name                  | yes      | string, no spaces                    |
+-----------------------+----------+--------------------------------------+
| description           | no       | string                               |
+-----------------------+----------+--------------------------------------+

Errors
------


+------------+-------------------------------------------------+------------------------------------+
| Error code | Error message                                   | Description                        |
+============+=================================================+====================================+
| 400        | error while creating agent group: <explanation> | See error message for more details |
+------------+-------------------------------------------------+------------------------------------+

Example request
---------------

::

   POST /1.1/agentgroups HTTP/1.1
   Host: xivoserver
   Accept: application/json
   Content-Type: application/json

   {
       "name": "carrental",
       "description": "Car rental return"
   }

Example response
----------------

::

   HTTP/1.1 201 Created
   Location: /1.1/agentgroups/1
   Content-Type: application/json

   {
       "id": 1,
       "name": "carrental",
       "description": "Car rental return"
       "links" : [
           {
               "rel": "agentgroups",
               "href": "https://xivoserver/1.1/agentgroups/1"
           }
       ]
   }

Update an Agent Group
=====================

Only the fields that need to be modified can be set.


Query
-----

::

   PUT /1.1/agentgroups/<id>

Input
-----

Same as for creating an agent group. Please see `Create an Agent Group`_


Errors
------

Same as for creating an agent group. Please see `Create an Agent Group`_


Example request
---------------

::

   PUT /1.1/agentgroups/29 HTTP/1.1
   Host: xivoserver
   Content-Type: application/json

   {
       "name": "accountdpt"
   }


Example response
----------------

::

   HTTP/1.1 204 No Content


Delete an Agent Group
=====================

An agent group can not be deleted if there are agents in the group.
Agents have to be removed from the group before.

Consult the documentation on :ref:`group-agent-associations`
for further details.


Query
-----

::

   DELETE /1.1/agentgroups/<id>

Errors
------

+------------+-----------------------------------------------------------------+-----------------------------------------+
| Error code | Error message                                                   | Description                             |
+============+=================================================================+=========================================+
| 400        | error while deleting Agent Group: <explanation>                 | See error message for more details      |
+------------+-----------------------------------------------------------------+-----------------------------------------+
| 400        | Error while deleting Agent Group: agent in group                | See explanation above                   |
+------------+-----------------------------------------------------------------+-----------------------------------------+
| 404        | Agent group with id=X does not exist                            | The requested agent group was not found |
+------------+-----------------------------------------------------------------+-----------------------------------------+

Example request
---------------

::

   DELETE /1.1/agentgroups/35 HTTP/1.1
   Host: xivoserver

Example response
----------------

::

   HTTP/1.1 204 No Content


Group Agent Association
=======================

See :ref:`group-agent-associations`.
