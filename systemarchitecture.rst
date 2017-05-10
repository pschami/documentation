System architecture
===================

.. image:: architecture-diagram.png
   :height: 700
   :width: 800
   :align: center

The architecture above consists of five key sections:

+----------------------------------------------+-----------------------------------------------------------------------------------------------------------+
|Section                                       |Desciption                                                                                                 | 
+==============================================+===========================================================================================================+
| Service Bus AMQP                             |This is the mechanism through which all the individual components in Dell Project Symphony communicate.    |
|                                              |It is an instance of RabbitMQ, an open source implementation of an AMQP  message broker, and is            |
|                                              |distributed by Pivotal under the Mozilla Public License.                                                   |
+----------------------------------------------+-----------------------------------------------------------------------------------------------------------+
|Symphony HAL and HAL Integration              |These provide a hardware abstraction layer between Project Symphony and the element managers for the       |
|                                              |components inside a Dell EMC Converged System.                                                             |
+----------------------------------------------+-----------------------------------------------------------------------------------------------------------+
|Portable, Autonomous Query Execution (PAQX)   |Defines a business capability that is available  through the API Gateway,for example assessing a           |
|                                              |Converged System against an RCM matrix. A PAQX is not required to have a public REST API. PAQX             |
|                                              |contracts are defined in a language-agnostic manner, as JSON schemas (http://json-schema.org/)             |
|                                              |and should be implementable by third-party developers without dependency on the Dell EMC software          |
|                                              |engineering teams. A PAQX can subscribe to events from other PAQX.                                         |
+----------------------------------------------+-----------------------------------------------------------------------------------------------------------+
|Core Services                                 |Includes credentials for the element managers and components and the configuration used to describe the    |
|                                              |components in a Converged System.  Also includes a registry of business capabilties so that additional     |
|                                              |PAQX can be deployed and consumed without restarting.                                                      |
+----------------------------------------------+-----------------------------------------------------------------------------------------------------------+
|Core Services API Gateway                     |API gateway is used for REST-based communication implementing Zuul + Consul.ioto act as a single entry     |
|                                              |point for all clients. The API gateway handles requests in one of two ways:                                |
|                                              |* Proxying/routing to the appropriate service.                                                             |
|                                              |* Orchestrating requests to the required services.                                                         |
+----------------------------------------------+-----------------------------------------------------------------------------------------------------------+

Core services breakdown
-----------------------

Endpoint registry
 The endpoint registry provides the ability to register new endpoints for software that can provide core functionality and/or data to the system. The endpoint registry is the repository of applications that the Converged system can leverage, for example, RackHD, CoprHD, vCenter

Capability registry
 Provides the ability to dynamically register and look up new business capabilities. The capability registry is the repository of functionality that the Converged System can provide, for example, Install ESXi..

System definition
 Allows storage, retrieval and querying of Converged Systems and system components.

Credentials
 Credential service is a secure repository of component and endpoint credentials. When HAL or PAQX request component credentials, the credentials are supplied in an encrypted format.

Element identity
 Identity service maps components between Project Symphony identifiers and third-party party identifiers such as CoprHD ViPR ID. This mapping allows correlation of components between PAQX.

PAQX 
-------

Defines a business capability that is available  through the API Gateway,for example assessing a Converged System against an RCM matrix. A PAQX is not required to have a public REST API. PAQX contracts are defined in a language-agnostic manner, as JSON schemas (http://json-schema.org/) and should be implementable by third-party developers without dependency on the Dell EMC software engineering teams. A PAQX can subscribe to events from other PAQX.

Field Replacement Unit (FRU) PAQX
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This PAQX automates the process of replacing a node (server with integrated storage mounted into a cabinet) in an operating Dell EMC VxRack FLEX System.. It  uses industry standard approaches to vacate existing virtual machines that might be running on the node, eliminating the node from any storage environment, and then replicating the configuration onto the replacement node, re-using existing identifiers such as IP addresses.

Dell Node Expansion PAQX 
~~~~~~~~~~~~~~~~~~~~~~~~

This PAQX automates the process of deploying a new node that has been racked into the cabinet of an already operating VxRack FLEX System.  It automates the expansion of the system and its available resources. 


