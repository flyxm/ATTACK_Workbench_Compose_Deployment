# ATT&CK_Workbench_Compose_Deployment                                       
 The ATT&amp;CK Workbench deployment project provides a complete solution that integrates frontend, REST API, MongoDB, Navigator, and Portainer container management tools, supporting rapid deployment and operation.

1、 Understand the core concepts first
ATT&CK Workbench: A tool for managing threat intelligence (STIX format data), similar to a "threat intelligence repository", that allows users to upload, edit, and delete threat data (such as attack methods, malware information, etc.) on their own.
ATT&CK Navigator: a visualization tool used to "visually display" threat intelligence (such as painting attack path maps).
ATT&CK Website: The official threat intelligence website of MITRE（
 https://attack.mitre.org/
 ）But you can also "replace" the official website content with your own Workbench data to become your own threat intelligence website.
2、 Deployment Objectives
Simply put, we need to do three things:

Set up Workbench (your threat intelligence repository).

Enable Navigator to read data from Workbench (using visualization tools to view threat data in the warehouse).

Enable custom ATT&CK Website to read Workbench data (display your threat data in the form of an official website).

ATT&CK provides a critical function that many organizations urgently need - to develop, organize, and use threat intelligence defense strategies in a standardized manner, enabling enterprise partners, industry personnel, and security vendors to communicate and interact in the same language.
