.. _group-agent-associations:

*************************
Group Agents Associations
*************************

Manage contact center agent groups, allowing an agent to be moved from one group to another.

An agent can be in only one group, a group can be empty.

An agent has to be in one group and therefore cannot be removed from a group, only moving it
from one group to another is permitted.

Agent Group Association Representation
======================================

+----------------+-------+-----------------------+
| Field          | Value | Description           |
+================+=======+=======================+
| agent_group_id | int   | Agent Group ID.       |
+----------------+-------+-----------------------+
| agent_id       | int   | Extension's ID.       |
+----------------+-------+-----------------------+

Get the agents of a group
=========================

Query
-----

::

    GET /agentgroup/agent_group_id/agents


Errors
------

+------------+---------------------------------------------------+-------------+
| Error code | Error message                                     | Description |
+============+===================================================+=============+
| 404        | group with id=<agent_group_id> does not exist     |             |
+------------+---------------------------------------------------+-------------+

Example Request
---------------

::

    GET /agentgroup/52/agents
    Host: xivoserver
    Accept: application/json


Example Response
----------------

::

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "total": 2,
        "items":
        [
            {
                "agent_group_id": 52,
                "agent_id": 7,
                "links": [
                    {
                        "rel": "agentgroups",
                        "href": "https://xivoserver/1.1/agentgroups/52"
                    },
                    {
                        "rel": "agents",
                        "href": "https://xivoserver/1.1/agents/7"
                    }
                ]
            },
            {
                "agent_group_id": 52,
                "agent_id": 11,
                "links": [
                    {
                        "rel": "agentgroups",
                        "href": "https://xivoserver/1.1/agentgroups/52"
                    },
                    {
                        "rel": "agents",
                        "href": "https://xivoserver/1.1/agents/11"
                    }
                ]
            }
        ]
    }

Move an Agent to a Group
========================

The agent will be removed from the group it belongs to and added to the new one.

.. note:: As an agent has to be always in a group, only moving an agent from one group
    to an other is allowed.

Query
-----

::
    POST /1.1/agentgroups/agent_group_id/agents

+-----------+----------+---------+------------------------+
| Field     | Required | Values  | Description            |
+===========+==========+=========+========================+
| agent_id  | yes      | int     | Must be an existing id |
+-----------+----------+---------+------------------------+

Errors
------

+------------+-------------------------------------------------------------------------------------+
| Error code | Error message                                                                       |
+============+=====================================================================================+
| 404        | Resource Not Found - Agent group was not found ('agent_group_id': <agent_group_id>) |
+------------+-------------------------------------------------------------------------------------+
| 400        | Input Error - field 'agent_id': Agent was not found                                 |
+------------+-------------------------------------------------------------------------------------+
