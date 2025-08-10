# ATT&CK_Workbench_Compose_Deployment                                       
 The ATT&amp;CK Workbench deployment project provides a complete solution that integrates frontend, REST API, MongoDB, Navigator, and Portainer container management tools, supporting rapid deployment and operation.

## Understand the core concepts first
### ATT&CK Workbench:     
A tool for managing threat intelligence (STIX format data), similar to a "threat intelligence repository", that allows users to upload, edit, and delete threat data (such as attack methods, malware information, etc.) on their own.
 It provides a user-friendly interface that facilitates centralized management and analysis of threat intelligence by security analysts and researchers.

### ATT&CK Navigator: 
A visualization tool used to "visually display" threat intelligence (such as painting attack path maps). Through a graphical interface, users can have a clearer understanding of the attacker's behavior patterns and attack paths, thereby better formulating defense strategies.

### ATT&CK Website: 
The official threat intelligence website of MITRE (<>), but you can also "replace" the website content with your own Workbench data to become your own threat intelligence website. This allows users to customize and display threat intelligence information according to their own needs.

### MongoDB: As a database component of ATT&CK Workbench, it is used to store threat intelligence data.
 It provides efficient data storage and retrieval capabilities, supporting fast access and management of large-scale data.

### Docker Compose: A tool for defining and managing multi container applications.
 In ATT&CK Workbench deployment, it simplifies the orchestration and startup process of front-end, back-end, database, and other services, making the deployment of the entire system more convenient and efficient.

### Portainer: 
A lightweight container management interface designed to simplify the management and monitoring of Docker containers. It provides an intuitive web interface for users to view and manage running resources such as containers, networks, volumes, etc., ensuring the stable operation of the entire ATT&CK Workbench system.

## Related technical documents:            
### [attack-workbench-frontend](https://github.com/center-for-threat-informed-defense/attack-workbench-frontend/)
### [attack-workbench-rest-api](https://github.com/center-for-threat-informed-defense/attack-workbench-rest-api)
### [attack-website](https://github.com/mitre-attack/attack-website/tree/master)
### [attack-navigator](https://github.com/mitre-attack/attack-navigator/)



## Overview of Deployment Architecture                                                   
Manage all containers through Docker Compose, build an isolated attack network environment, and ensure communication between services through service names.
 The core components include:

### Attack workbench frontend:  
Front end interface that calls REST API.

### Attack workbench rest API:  
Provides API interface to connect to MongoDB.

### MongoDB:  
Stores ATT&CK Workbench data. 

### Attack navigator:  
An independent MITRE ATT&CK navigation tool (standalone service).

### Portainer: 
Container Management Panel (optional for monitoring and managing containers).

### Docker Compose:  
A tool for defining and managing multi container applications.

## Requirements

## Verification Services

Attack workbench frontend： https://localhost:8080 (accessed through a browser)

Attack workbench rest API： http://localhost:3000 (accessed through a browser)

MongoDB： http://localhost:27017 (Using MongoDB client tools)

Attack navigator： http://localhost:4200 (accessed through a browser)

Portainer： http://localhost:9000 (accessed through a browser)
 
