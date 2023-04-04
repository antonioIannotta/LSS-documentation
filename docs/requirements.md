---
title: Requirements
has_children: false
nav_order: 3
---

# Requirements

## Business requirements

| ID | Requirement |
| -- | ----------- |
| BR1 | The application should provide an easy way for parking slot owners to manage their slots. |

## User requirements

| ID | Requirement |
| -- | ----------- |
| UR1 | Users should be able to create a new account by providing a name, an email, and a password. |
| UR2 | Users should be able to log in to their account using their email and password. |
| UR3 | Users should be able to change their password. |
| UR4 | Users should be able to delete their account and have all associated data removed from the system. |
| UR5 | Users should be able to search for available parking slots near their current location. |
| UR6 | Users should be able to reserve a parking slot for a specific time period. |
| UR7 | Users should be able to extend the occupation of their parking slot. |
| UR8 | Users should be able to terminate the parking slot occupation. |

## Functional requirements

| ID | Requirement |
| -- | ----------- |
| FR1 | The system should allow users to register by providing name, email and password. |
| FR2 | The system should authenticate registered users by verifying their login credentials (email and password). |
| FR3 | The system should allow users to change their password at any time. |
| FR4 | The system should allow users to delete their account. |
| FR5 | The system should allow users to log out. |
| FR6 | The system should allow users to occupy a parking slot by selecting it from a map and specifying the end of stop. |
| FR7 | The system should allow users to extend the occupation of a parking slot for a specified period. |
| FR8 | The system should show the user the state of the parking slots near him. |
| FR9 | The system should show the user the parking slot which is currently occupied by them. |
| FR10 | The system should allow users to terminate the occupation of a parking slot. |

## Non functional requirements

| ID | Requirement |
| -- | ----------- |
| NFR1 | The system should be easy to maintain and update. |
| NFR2 | The system should be able to scale horizontally in response to changing user demand. |
| NFR3 | The system response times should be fast enough to provide a smooth user experience. |

## Implementation requirements

| ID | Requirement |
| -- | ----------- |
| NFR1 | The system should be designed using a modular architecture, following the principles of Domain-Driven Design (DDD). This includes clearly defined domain entities, services, and repositories. |
| NFR2 | The application should provide a RESTful API for accessing and modifying data. The API should use standard HTTP methods. |
| NFR3 | The deployment process of the backend and of the frontend should be automated. |
