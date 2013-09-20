.. _device-line-association-api:

***********************
Device-Line Association
***********************

Associate a Line to a Device
============================

After associating a line, the device needs to be synchronized for the changes to take effect. Please
see :ref:`synchronize-device-api`.

**Parameters**

line_id
    Line's id

::

    POST /1.1/lines/<line_id>/device

**Input**

+-----------+----------+--------+-------------------------------+
| Field     | Required | Values | Description                   |
+===========+==========+========+===============================+
| device_id | yes      | string | Must be an existing device id |
+-----------+----------+--------+-------------------------------+

**Example request**::

    POST /1.1/lines/59/device
    Host: xivoserver
    Content-Type: application/json

    {
        "device_id": "412c212cff500cc158f373ff00e078f7",
    }

**Example response**::

    HTTP/1.1 204 No Content



Remove a line from a device
===========================

After removing a line, the device needs to be synchronized for the changes to take effect. Please
see :ref:`synchronize-device-api`.

**Parameters**

line_id
    Line's id

::

    DELETE /1.1/lines/<line_id>/device

**Example request**::

    DELETE /1.1/lines/20/device
    Host: xivoserver

**Example response**::

    HTTP/1.1 204 No Content
