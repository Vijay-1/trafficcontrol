..
..
.. Licensed under the Apache License, Version 2.0 (the "License");
.. you may not use this file except in compliance with the License.
.. You may obtain a copy of the License at
..
..     http://www.apache.org/licenses/LICENSE-2.0
..
.. Unless required by applicable law or agreed to in writing, software
.. distributed under the License is distributed on an "AS IS" BASIS,
.. WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
.. See the License for the specific language governing permissions and
.. limitations under the License.
..

.. _to-api-v2-servers-details:

*******************
``servers/details``
*******************
Retrieves details of :ref:`tp-configure-servers`.


``GET``
=======
:Auth. Required: Yes
:Roles Required: None
:Response Type:  Array

.. note:: On top of the response including the response key that is of type array it will also include the keys ``limit``, ``orderby``, and ``size``.

Request Structure
-----------------
.. table:: Request Query Parameters

	+----------------+----------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
	| Name           | Required                               | Description                                                                                                                                                    |
	+================+========================================+================================================================================================================================================================+
	| hostName       | Required if no physLocationID provided | Return only the servers with this (short) hostname. Capitalization of "hostName" is important.                                                                 |
	+----------------+----------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
	| physLocationID | Required if no hostName provided       | Return only servers with this integral, unique identifier for the physical location where the server resides. Capitalization of "physLocationID" is important. |
	+----------------+----------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. code-block:: http
	:caption: Request Example

	GET /api/2.0/servers/details?hostName=edge HTTP/1.1
	User-Agent: python-requests/2.22.0
	Accept-Encoding: gzip, deflate
	Accept: */*
	Connection: keep-alive
	Cookie: mojolicious=...

Response Structure
------------------
:limit:         The maximum size of the ``response`` array, also indicative of the number of results per page using the pagination requested by the query parameters (if any) - this should be the same as the ``limit`` query parameter (if given)
:orderby:       A string that names the field by which the elements of the ``response`` array are ordered - should be the same as the ``orderby`` request query parameter (if given)
:response:      An array of objects, each of which represents the details of a given :ref:`Server <tp-configure-servers>`.

	:cachegroup:            A string that is the :ref:`name of the Cache Group <cache-group-name>` to which the server belongs
	:cdnName:               Name of the CDN to which the server belongs
	:deliveryservices:      An array of integral, unique identifiers for :term:`Delivery Services` to which this server belongs
	:domainName:            The domain part of the server's :abbr:`FQDN (Fully Qualified Domain Name)`
	:guid:                  An identifier used to uniquely identify the server

		.. note::       This is a legacy key which only still exists for compatibility reasons - it should always be ``null``

	:hostName:              The (short) hostname of the server
	:httpsPort:             The port on which the server listens for incoming HTTPS connections/requests
	:id:                    An integral, unique identifier for this server
	:iloIpAddress:          The IPv4 address of the server's :abbr:`ILO (Integrated Lights-Out)` service\ [1]_
	:iloIpGateway:          The IPv4 gateway address of the server's :abbr:`ILO (Integrated Lights-Out)` service\ [1]_
	:iloIpNetmask:          The IPv4 subnet mask of the server's :abbr:`ILO (Integrated Lights-Out)` service\ [1]_
	:iloPassword:           The password of the of the server's :abbr:`ILO (Integrated Lights-Out)` service user\ [1]_ - displays as simply ``******`` if the currently logged-in user does not have the 'admin' or 'operations' :term:`Role(s) <Role>`
	:iloUsername:           The user name for the server's :abbr:`ILO (Integrated Lights-Out)` service\ [1]_
	:interfaceMtu:          The :abbr:`MTU (Maximum Transmission Unit)` to configured on ``interfaceName``
	:interfaceName:         The name of the primary network interface used by the server
	:ip6Address:            The IPv6 address and subnet mask of ``interfaceName``
	:ip6Gateway:            The IPv6 address of the gateway used by ``interfaceName``
	:ipAddress:             The IPv4 address of ``interfaceName``
	:ipGateway:             The IPv4 address of the gateway used by ``interfaceName``
	:ipNetmask:             The IPv4 subnet mask used by ``interfaceName``
	:mgmtIpAddress:  The IPv4 address of the server's management port
	:mgmtIpGateway:  The IPv4 gateway of the server's management port
	:mgmtIpNetmask:  The IPv4 subnet mask of the server's management port
	:offlineReason:         A user-entered reason why the server is in ADMIN_DOWN or OFFLINE status
	:physLocation:          The name of the physical location where the server resides
	:profile:               The :ref:`profile-name` of the :term:`Profile` used by this server
	:profileDesc:           A :ref:`profile-description` of the :term:`Profile` used by this server
	:rack:  A string indicating "server rack" location
	:routerHostName:        The human-readable name of the router responsible for reaching this server
	:routerPortName:        The human-readable name of the port used by the router responsible for reaching this server
	:status:                The status of the server

		.. seealso::    :ref:`health-proto`

	:tcpPort: The port on which this server listens for incoming TCP connections

		.. note::       This is typically thought of as synonymous with "HTTP port", as the port specified by ``httpsPort`` may also be used for incoming TCP connections.

	:type:                  The name of the 'type' of this server
	:xmppId:                An identifier to be used in XMPP communications with the server - in nearly all cases this will be the same as ``hostName``
	:xmppPasswd:            The password used in XMPP communications with the server

:size:          The page number - if pagination was requested in the query parameters, else ``0`` to indicate no pagination - of the results represented by the ``response`` array. This is named "size" for legacy reasons

.. code-block:: http
	:caption: Response Example

	HTTP/1.1 200 OK
	Access-Control-Allow-Credentials: true
	Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept, Set-Cookie, Cookie
	Access-Control-Allow-Methods: POST,GET,OPTIONS,PUT,DELETE
	Access-Control-Allow-Origin: *
	Content-Encoding: gzip
	Content-Type: application/json
	Set-Cookie: mojolicious=...; Path=/; Expires=Mon, 24 Feb 2020 01:27:36 GMT; Max-Age=3600; HttpOnly
	Whole-Content-Sha512: HW2F3CEpohNAvNlEDhUfXmtwpEka4dwUWFVUSSjW98aXiv10vI6ysRIcC2P9huabCz5fdHqY3tp0LR4ekwEHqw==
	X-Server-Name: traffic_ops_golang/
	Date: Mon, 24 Feb 2020 00:27:36 GMT
	Content-Length: 493

	{
		"limit": 1000,
		"orderby": "hostName",
		"response": [
			{
				"cachegroup": "CDN_in_a_Box_Edge",
				"cdnName": "CDN-in-a-Box",
				"deliveryservices": [
					1
				],
				"domainName": "infra.ciab.test",
				"guid": null,
				"hardwareInfo": null,
				"hostName": "edge",
				"httpsPort": 443,
				"id": 5,
				"iloIpAddress": "",
				"iloIpGateway": "",
				"iloIpNetmask": "",
				"iloPassword": "",
				"iloUsername": "",
				"interfaceMtu": 1500,
				"interfaceName": "eth0",
				"ip6Address": "fc01:9400:1000:8::3",
				"ip6Gateway": "fc01:9400:1000:8::1",
				"ipAddress": "172.16.239.3",
				"ipGateway": "172.16.239.1",
				"ipNetmask": "255.255.255.0",
				"mgmtIpAddress": "",
				"mgmtIpGateway": "",
				"mgmtIpNetmask": "",
				"offlineReason": "",
				"physLocation": "Apachecon North America 2018",
				"profile": "ATS_EDGE_TIER_CACHE",
				"profileDesc": "Edge Cache - Apache Traffic Server",
				"rack": "",
				"routerHostName": "",
				"routerPortName": "",
				"status": "REPORTED",
				"tcpPort": 80,
				"type": "EDGE",
				"xmppId": "edge",
				"xmppPasswd": ""
			}
		],
		"size": 1
	}

.. [1] For more information see the `Wikipedia page on Lights-Out management <https://en.wikipedia.org/wiki/Out-of-band_management>`_\ .
