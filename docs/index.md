# Smart parking

## Introduction

### Overview
The parking situation it's etimated to become more and more complicated in the next years. This increase of complexity is due to two main factors:
* the need for the cities to earn moeny from the available parking slot
* the number of cars in the city
These factors lead to the possibility for the citizen to trust on a parking system fully digitalized in which the payment system is secure and the availability of a certain parking slot is always shown in a reliable manner.

In this project the payment system issue has not been addressed.

### Requirements
In the following are reported the main requirements that the system must satisfy:
* The user must easily access to the system
* The system must show to the user the state of the several parking slot around the city.
* The system must allow the user to select the end of the stop after having parked the car/motorcycle.
* The system must allow the user to easily increment the stop duration

## Domain Analysis

### Knowledge crunching

In order to deal with the complexity of the project there have been a continuous exchange of information with the stakeholders. More specifically the information related to the system and the knowledge crunching process itself have been implementet through interviews.

* **How does a user access to the system?**\
The sign-up and sign-in operations are handled through the Google Auth tool. So the user only needs to have a Google account

* **What is a park?**\
In this application it's better to deal with the concept of park in the meaning of single slot where a certain person can park its car/motorcycle. So the user can park its car/motorcycle into a parking slot.

* **What does it happen when a user wants to park its car/motorcycle?**\
The user sees a free parking slot and occupy it with its car/motorcycle. After that from the app he/she selects the hour in which the stop will end.

* **How can a user see the parking slots status around the city?**\
The application shows every parking slot that is in a certain place. The place depends on the location of the user and on the distance of interest that the user specify. Every parking slot is shown in one of three possible colors: 
    - **red**: the parking slot is busy
    - **orange**: the parking slot will be free in few minutes
    - **green**: the parking slot is free

* **How does a user select the distance of interest in which he can see the parking slots?**\
The user can select the distance of interest specifying a range in which he wants to see the parking slots status.

* **Could a user increment the stop?**\
The user can increment the duration of the stop before the previous stop ends. More specifically he/she can only specify a time after the end stop time previously inserted.

### Ubiquitous language

The result of the previous knowledge crunching step has been the creation of a glossary with the goal to provide an ubiquitous language around the which develop the whole system. The glossary contains the following concepts:
* **Parking slot**: location where the user can park its own car/motorcycle
* **Occupy**: the action of moving a vehicle into a parking slot
* **Application**: Android application
* **End stop**: the time limit within the user must remove its vehicle from the parking slot.

### Use cases
After a proper domain analysis has been possible to detect the use cases and the entities that are involved into this system. The following imgage illustrates the uses cases for Smart Parking: 
![alt text](../use_case/Use%20case%20diagram.drawio.png "Use cases") \
From the previous image is possible to see how in this system there are two main actors involved in the operations:\
* **User**: the user is the actor that is intended to use the application in order to look for free parking slot, to set an end stop time and to increment this time.
* **Client**: the client is the actor in charge to handle all the requests that are made by the user. More specifically the client is intended to handle the operation of registration and access to the system. The client is also in charge to handle a proper visualization of the status of the parking slots. Is also the client that is in charge to handle the setting of the end stop time or the increment of this time itself. \
It's possible to notice, from the picture, the following operation:
* **Access**: for the access operation the directionality is from the user to the client for the request and every possible check is handled by the client. \
* **Check parking slots status**: in this case is the client that is in charge to periodically show to the user the status of the parking slots.
* **Park**: this operation involves the user that is involved into the selection of the parking slot that he wants to occupy and into the setting of the end time and, at the same time, involves the client that is in charge to change the status of the specified parking slot.
* **Extend stop**: with this operation the user extends the end stop time and the client is in charge to handle this extension by meaning of properly change the status of the parking slot. For example turning the color of a certain parking slot from orange to red.

### User stories
After the collaction of the main use cases for the system have been collected some user stories that describe the operativity of the system under certain circumstances. More specifically the user stories illustrated are the following:
* The user access to the system
* The user park its own car
* The user checks for the status of parking slots in the map
* The user extends its stop
In the following are illustrated the user stories:

![alt text](../user%20stories/The%20user%20access%20to%20the%20system_2023-02-07.png "User stories access")

![alt text](../user%20stories/Parking%20operation_2023-02-07.png "Parking user story")

![alt text](../user%20stories/Status%20show_2023-02-07.png "Show parking slot status")

![alt text](../user%20stories/Parking%20extension_2023-02-07.png "Parking extension")

### Strategic Design

### Tactical Design

## Design

### Architecture and implementation
The architecture decided to adopt is a simple Client/Server architecture, with the client (the Android app) that is in charge to handle the interaction with the user and thge backend (the server) that is in charge to provide consistent data related to the parking slots and that is in charge to handle the registration/access procedures. \
A first general consideration is meaningful considering the separation between the logic/storage component (the backend) and the logic/presentation component (the frontend). This separation has been the result of a deep analysis and it resulted as the best choise because of the possibility to have independence in both architecture and implementation of each part. More specifically, adopting this philosophy of separation of concerns at the architectural side has been possible to choose the database that turned out to be the best for us and to choose the web service framework totally free.
It's important to have a deeper view on both side, frontend and backend, in order to better analyze their own architecture. \
#### Backend
The backend is composed by the database and by the software that is in charge to define the logic before the access to the database. More specifically there are two main component:
* **Database**
* **Web Service**
While there's not too much to say about the database it's important to talk about the web service. There are two main routes:
* **/user**
* **/parking-slot**
Each of these routes is in charge to recall logic to handle either the user access/registration to the service or to retrieve/update from/to the database the infromation related to the parking slots.

##### Backend implementation
There have been several theoretical consideration that led to choose a specific typology of either database or web framework rather than the another one.
* **Database**: the choise of a NoSQL address has been the result of a simple consideration: since the data in this application have a core into the parking slots it has been wise to map this core even into the database. Adopting a NoSQL database like **MongoDB** made it easier to organize the several documents indexing each one by the parking slot identifier.
* **Web Service**:  
#### Client

### CI/CD

## Installation