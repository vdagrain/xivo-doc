.. _agent-queue-association:

***********************
Agent Queue Association
***********************

.. warning:: This service is experimental

Service for associating an agent with a queue.


Association Representation
==========================

Description
-----------

+----------+-------+-------------------+
| Field    | Value | Description       |
+==========+=======+===================+
| agent_id | int   | Agent's ID        |
+----------+-------+-------------------+
| queue_id | int   | The Queue's id    |
+----------+-------+-------------------+
| penalty  | int   | The penalty value |
+----------+-------+-------------------+


Get an Agent - Queue association
================================

Query
-----

::

    GET /1.1/queues/<queue_id>/members/agents/<agent_id>


Errors
------

+------------+-------------------------------------------------------------------------------------------------+-------------+
| Error code | Error message                                                                                   | Description |
+============+=================================================================================================+=============+
| 404        | Resource Not Found - QueueMember was not found ('agent_id': <agent_id>, 'queue_id': <queue_id>) |             |
+------------+-------------------------------------------------------------------------------------------------+-------------+
| 404        | Resource Not Found - Queue was not found ('queue_id': <queue_id>)                               |             |
+------------+-------------------------------------------------------------------------------------------------+-------------+
| 404        | Resource Not Found - Agent was not found ('agent_id': <agent_id>)                               |             |
+------------+-------------------------------------------------------------------------------------------------+-------------+


Example request
---------------

::

    GET /1.1/queues/3/members/agents/18
    Host: xivoserver


Example response
----------------

::

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "agent_id": 18,
        "queue_id": 3,
        "penalty": 5
    }

Associate an Agent to a Queue
=============================

Position in queue is set to max position + 1 or 0 if it is the first member of the queue

Query
-----

::

    POST /1.1/queues/<queue_id>/members/agents

Input
-----

+-----------+----------+---------+------------------------+
| Field     | Required | Values  | Description            |
+===========+==========+=========+========================+
| agent_id  | yes      | int     | Must be an existing id |
+-----------+----------+---------+------------------------+
| penalty   | yes      | int     | >  0                   |
+-----------+----------+---------+------------------------+

Errors
------

+------------+---------------------------------------------------------------------+-------------+
| Error code | Error message                                                       | Description |
+============+=====================================================================+=============+
| 404        | Resource Not Found - Queue was not found ('queue_id': <queue_id>)   |             |
+------------+---------------------------------------------------------------------+-------------+
| 400        | Input Error - field 'agent_id': Agent was not found                 |             |
+------------+---------------------------------------------------------------------+-------------+
| 400        | Resource Error - Agent is associated with a Queue                   |             |
+------------+---------------------------------------------------------------------+-------------+


Example request
---------------

::

    POST /1.1/queues/3/members/agents
    Host: xivoserver
    Content-Type: application/json

    {
        "agent_id" : 32,
        "penalty": 12
    }

Example response
----------------

::

    HTTP/1.1 201
    Location: /1.1/queues/3/members/agents/32

    {
        "agent_id": 32,
        "queue_id": 3,
        "penalty": 12
    }

Remove Agent from a Queue
=========================

Query
-----

::

    DELETE /1.1/queues/<queue_id>/members/agents/<agent_id>

Errors
------

+------------+-------------------------------------------------------------------------------------------------+-------------+
| Error code | Error message                                                                                   | Description |
+============+=================================================================================================+=============+
| 404        | Resource Not Found - QueueMember was not found ('agent_id': <agent_id>, 'queue_id': <queue_id>) |             |
+------------+-------------------------------------------------------------------------------------------------+-------------+
| 404        | Resource Not Found - Queue was not found ('queue_id': <queue_id>)                               |             |
+------------+-------------------------------------------------------------------------------------------------+-------------+
| 404        | Resource Not Found - Agent was not found ('agent_id': <agent_id>)                               |             |
+------------+-------------------------------------------------------------------------------------------------+-------------+

Example request
---------------

::

    DELETE /1.1/queues/3/members/agents/18
    Host: xivoserver


Example response
----------------

::

    HTTP/1.1 204 NO CONTENT


Edit an Agent - Queue association
=================================

Query
-----

::

    PUT /1.1/queues/<queue_id>/members/agents/<agent_id>


Errors
------

+------------+-------------------------------------------------------------------------------------------------+-------------+
| Error code | Error message                                                                                   | Description |
+============+=================================================================================================+=============+
| 404        | Resource Not Found - QueueMember was not found ('agent_id': <agent_id>, 'queue_id': <queue_id>) |             |
+------------+-------------------------------------------------------------------------------------------------+-------------+
| 404        | Resource Not Found - Queue was not found ('queue_id': <queue_id>)                               |             |
+------------+-------------------------------------------------------------------------------------------------+-------------+
| 404        | Resource Not Found - Agent was not found ('agent_id': <agent_id>)                               |             |
+------------+-------------------------------------------------------------------------------------------------+-------------+


Example request
---------------

::

    PUT /1.1/queues/3/members/agents/18
    Host: xivoserver
    Content-Type: application/json
    
    {
        "penalty": 5
    }
    

Example response
----------------

::

    HTTP/1.1 204 OK

