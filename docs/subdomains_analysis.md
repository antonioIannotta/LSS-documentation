---
title: Subdomains Analysis
has_children: false
nav_order: 4
---

# Subdomains Analysis

In Domain Driven Design, in order to manage the complexity of the domain you want to analyze better, it is advisable to thoroughly explore the problem-domain by identifying subdomains.
We identified three subdomains:
* **Parking Management (Core)**: this is responsible for managing parking slots, their availability, and their occupancy status. It contains entities such as Parking Slot and Parking Slot State. It is a core subdomain because it contains the main entities and business logic related to the problem: this is the central domain of the application.
* **User Management (Generic)**: this is responsible for managing users, sign up, and login. It contains entities such as User, User Credentials, and Authentication Token. This subdomain is generic because the functionalities it provides are often needed in many different applications and are not specific to the domain of parking management.
* **Client (Supporting)**: this is responsible for handling user interactions, displaying information and sending requests to the backend. It is a supporting subdomain because it provides the user interface and handles user interactions, but it does not contain any business logic related to parking or user management. Its main role is to interact with the other bounded contexts to fulfill user requests and present information to the user.

![alt text](https://github.com/antonioiannotta/LSS-documentation/blob/main/strategic_design/core_domain_chart.png?raw=true "Core Domain Chart")

## Bounded contexts

The subdomain **Parking Management** is made up of two bounded contexts, which are described in the following bounded context canvases:

| Name | Parking Slot Management |
| Description | Responsible for managing parking slots and their availability. Contains entities such as Parking Slot and Parking Slot State. |
| Domain Roles | execution context |
| Strategic Classification - Domain | Core |
| Strategic Classification - Business Model | - |
| Strategic Classification - Evolution | Custom built |
| Ubiquitous Language | Parking slot, Parking slot status, Range of interest, Vehicle |
| Business Decisions | Possibility to add parking slots |
| Inbound Communications | - |
| Outbound Communication | User Management |

| Name | Parking Slot Occupation Management |
| Description | Responsible for managing parking slot occupation, including incrementing and freeing parking slots. |
| Domain Roles | execution context |
| Strategic Classification - Domain | Core |
| Strategic Classification - Business Model | Revenue generator |
| Strategic Classification - Evolution | Custom built |
| Ubiquitous Language | Occupied parking slot, Park, Parking slot, Parking slot status, Vehicle, Stop end |
| Business Decisions | - |
| Inbound Communications | - |
| Outbound Communication | Parking Slot Management |

The subdomain **User Management** is made up of of two bounded contexts, which are described in the following bounded context canvases:

| Name | User Management |
| Description | responsible for managing user and personal information. Contains entities such as User. |
| Domain Roles | execution context |
| Strategic Classification - Domain | Generic |
| Strategic Classification - Business Model | - |
| Strategic Classification - Evolution | Product |
| Ubiquitous Language | Login, Sign Up, User |
| Business Decisions | - |
| Inbound Communications | - |
| Outbound Communication | Authentication |

| Name | Authentication |
| Description | responsible for managing user authentication and authorization. Contains entities such as User Credentials. |
| Domain Roles | execution context |
| Strategic Classification - Domain | Generic |
| Strategic Classification - Business Model | Revenue generator |
| Strategic Classification - Evolution | Product |
| Ubiquitous Language | Login, Sign up, User credentials |
| Business Decisions | - |
| Inbound Communications | - |
| Outbound Communication | User Management |

The supporting subdomain **Client** is made up of of two bounded contexts, which are described in the following bounded context canvases:

| Name | User Interface |
| Description | responsible for handling user interactions and presenting information to the user. |
| Domain Roles | execution context |
| Strategic Classification - Domain | Supporting |
| Strategic Classification - Business Model | Engagement creator |
| Strategic Classification - Evolution | Custom built |
| Ubiquitous Language | Duration of parking, Free parking slot, Login, Occupied parking slot, Park, Parking slot, Parking slots map, Parking slot marker, Parking slot status, Range of interest, Sign up, User, Vehicle, Stop end |
| Business Decisions | The user interface should be localizable in different languages |
| Inbound Communications | - |
| Outbound Communication | Network Communication bounded-context |

| Name | Network Communication |
| Description | responsible for communication with the backend server, including sending and receiving data. |
| Domain Roles | execution context |
| Strategic Classification - Domain | Supporting |
| Strategic Classification - Business Model | - |
| Strategic Classification - Evolution | Commodity |
| Ubiquitous Language | Frontend, Backend |
| Business Decisions | - |
| Inbound Communications | - |
| Outbound Communication | Authentication, User Management, Parking slot, Parking slot management |


## Context Map
The bounded contexts relationship was modeled in a context map. The context map was generated using Context Mapper VSC's extension.
![alt text](https://github.com/antonioiannotta/LSS-documentation/blob/main/strategic_design/cm.png?raw=true "System context map")

