---
title: Design
has_children: false
nav_order: 5
---

# Design

## Architecture and implementation
The architecture decided to adopt is a simple Client/Server architecture, with the client (the Android app) that is in charge to handle the interaction with the user and the backend (the server) that is in charge to provide consistent data related to the parking slots and that is in charge to handle the registration/access procedures. \
A first general consideration is meaningful considering the separation between the logic/storage component (the backend) and the logic/presentation component (the frontend). This separation has been the result of a deep analysis and it resulted as the best choise because of the possibility to have independence in both architecture and implementation of each part. More specifically, adopting this philosophy of separation of concerns at the architectural side has been possible to choose the database that turned out to be the best for us and to choose the web service framework totally free.
It's important to have a deeper view on both side, frontend and backend, in order to better analyze their own architecture.

### Backend
The backend is composed by the database and by the software that is in charge to define the logic behind the access to the database. More specifically there are two main component:
* **Database** two different databases was used for the application, one for the user authentication bounded context and one for the parking slot management bounded context. Databases was each stored on MongoDB Cloud.
* **Web Service**
While there's not too much to say about the database it's important to talk about the web service. There are two main routes:
* **/user**
* **/parking-slot**
Each of these routes is in charge to recall logic to handle either the user access/registration to the service or to retrieve/update from/to the database the infromation related to the parking slots.

#### Backend: general concepts
There have been several theoretical consideration that led to choose a specific typology of either database or web framework rather than the another one.
* **Database**: the choise of a NoSQL address has been the result of a simple consideration: since the data in this application have a core into the parking slots it has been wise to map this core even into the database. Adopting a NoSQL database like **MongoDB** made it easier to organize the several documents indexing each one by the parking slot identifier.
* **Web Service**: the choise of the web framework to use for the backend has been led by the consideration of have a single language across the whole application. In order to achieve this goal has been used Ktor as web framework (implemented in Kotlin). In order to implement the routes for the web service has been adopted a ReST approach in order to create ReSTful API. \

#### Backend: architecture
The architecture adopted for the backend has followed the rules of the **Clean architecture**. This architecture led to the detection of several layers for the application and has been applied to both the two modules that the backend has: **user** and **parking slot**. The layers detected are the following:
* **entity**
* **use cases**
* **interface adapter**
* **framework**

In the following is reported the architecture for each module:

##### User 
* Entity:
    - **User**: completely mapped in the database, it represents all used information about the user (email, password and name).
    - **User credentials**: represents a id-password pair used from the user to log him into the system (the user identifier is the email address).
    - **User info**: objects that represent the user's not sensitive information. It corresponds to a user object without password field.
* Use cases:
    - **Login**: a user provides his identified and password to access the system.
    - **create user**: a user wants to register him on the sistem.
    - **Get info**: a user wants see him account information.
    - **Change password**: a user wants to change his password with a new one. 
    - **Delete user**: a registered user wants to delete his account.
    - **Validate**: a email-password pair must be validated (checking if a user with this email exists in the database and if the associated password corresponds with the one in the email-password pair).
    - **Exists** an user email must be verified checking in the database if there's a registered user with this email

* Interface adapter:
    - handles the framework layer requests, translating them into use cases.

* Framework:
    - It's represented by KTOR web server and Mongo database.

##### Parking slot
* Entity:
    - **Paring Slot**: this entity is completely mapped into the database and      represents a single parking slot. Any other component of the parking slot module depends on the parking slot entity.

* Use cases:
    - **Occupy a slot**: this is the use case in which a user is intended to occupy a parking slot for a certain amount of time
    - **Increment occupation**: this is the use case in which a user is intended to extend the occupation of a certain parking slot.
    - **Free slot**: this is the use case in which a user free the parking slot previously occupied.

* Interface adapter:
    - This layer is in charge to handle the requests coming from the framework, that is the most external layer, and translate them into use cases.

* Framework:
    - In this case is represented by the web server and by the database choose for the purpose.

##### User

The 3 inner layers of CLEAN architecture are located in **user** submodule while the framework layer is in the core module due to the shared aspect with **parking-slot** submodule (there are framework features shared by both submodules, such as the application server and routing and authentication features). 

* **entity** package contains serializable data classes that implement user entities (User, UserInfo and UserCredentials).
* **use_cases** package contains an interfaace that describe the behaviour of all the use cases.
* **interface_adapter** package contains a class UserInterfaceAdapter that implements the use cases. It also includes two submodules: a model package and a utils package. The model subpackage contains data classes that define the request/response body structure that should be used from the framework layer. The utils subpackage contains utility functions such as email sender method and jwt handler methods (generation and validation).

##### Parking Slot
In order to talk about the implementation of the parking slot operations inside the submodule **parking-slot** is important to keep together implementation and clean architecture. The clean architecture previously presented is composed of four layers. The three inner layers (entity, use cases and interface adapter) correspond to three packages into the **parking-slot** submodule. The fourth layer (framework) is outside this module by the moment it is common to both **parking-slot** and **user** submodules. In the following are reported more specifically the element that fill any of the three layers of the **parking-slot** submodule:
* **entity**: in this package there are all those entities that are the domain basis for the parking-slot subproject. These core entities are:
    - **Position**: represents a single position expressed as latitude-longitude coordinates. This entity is helpful because it provides the exact location for each parking slot.
    - **Center**: represents the area around the which the user is interested about the status of the parking slots.
    - **StopEnd**: represents the end stop setted or incremented for a certain parking slot.
    - **ParkingSlot**: represents a single parking slot with its properties: the status (either occupied or not), the position expressed as latitude and longitude, the end stop time and the identifier (that is unique for each parking slot)
    All these entities are modelled as **Kotlin** classes. This choise derived by the consideration about the possible inheritance of these classes. By the moment these entities could be subjected to inheritance in future uses and future development of the domain the choise has been to model them as classes.

* **use_cases**: in this package are reported the use cases, representing the second layer of the clean architecture previosuly presentend. Remaining on the implementation side the use cases have been represented as method of an interface to be implemented by the interface adapter. This is because the interface adapted is in charge to implement the methods translating into it the framwework requests.

* **interface_adapter**: this package contains a class, **InterfaceAdapter**, that receives as constuctor parameter a collection of the MongoDB database, in this case. This class implements the UseCases interface and provide the results that are used into the framework package to provide the responds to the incoming requests from the client. It's important to notice how, implementing the Interface adapter in this way, the use cases remains independent from it and, in case of change of the Database, will only be necessary to change the constructor argument and some operation inside this class. This solution has been thought to be aligned to the SoC principle and, furthermore, to be coherent with the previous domain analysis.

### Frontend
The frontend follows the Clean Architecture with a separation into four main layers:
* The **domain layer** contains all the entities, use cases and repositories. It defines the core concepts and business rules of the app and use cases. The domain layer does not depend on any other layers.
* The **data layer** is responsible for managing the interaction with local storage, backend API and system services. It contains data sources, and repositories implementation. This layer only depends on the domain layer.
* The **presentation layer** contains the user interface components and provides the main user-facing features of the app. This module only depends on the domain layer.
* The **app layer** depends on all the three previous layers and it is responsible for the launch of the app.

By separating the frontend into distinct layers, we can ensure that the different parts of the app are decoupled and can be developed and tested independently. Both the data layer and the presentation layer only depends on interfaces (Dependency Inversion Principle). To give the proper implementation to the layers that needs it we will use the service locator pattern.


_In the following UML Class Diagrams it is possible to find the `async` keyword: it means that the method doesn't return immediately: it is asynchronous._

#### Domain Layer
The domain layer is furtherly split down in `user` and `parking slot` packages to isolate the DDD subdomains.

##### Entities

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/design/entities_parking_slot.png?raw=true"/>
  <figcaption>Parking slot package entities</figcaption>
</figure>
<br/>
In the above UML Class Diagram there are multiple entities used to represent the parking slot. This is identified by an `id` and contains information about its position and its state (whether it's occupied or not).

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/design/entities_parking_slot.png?raw=true"/>
  <figcaption>User package entities</figcaption>
</figure>
<br/>
In the above UML Class Diagram there are several classes used to represent the user. In particular:

* **User**: a user registered in the system;
* **NewUser**: a new user to be added in the system;
* **UserCredentials**: the credentials to be used to login in the system;
* **AuthState**: whether an user is currently logged in or not.

<br/>

##### Repositories
<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/design/repositories_user.png?raw=true"/>
  <figcaption>User Repository</figcaption>
</figure>
<br/>

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/design/repositories_parking_slot.png?raw=true"/>
  <figcaption>Parking Slot Repository</figcaption>
</figure>
<br/>

##### Use Cases
<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/design/base_usecases.png?raw=true"/>
  <figcaption>Base use cases</figcaption>
</figure>
<br/>

To simplify the creation of use cases we have created some abstract classes from which all the use cases inherits.
* `UseCase`: a non-failable use case that always returns in a non-asynchronous manner
* `AsyncUseCase`: a non-failable asynchronous use case
* `AsyncFailableUseCase`: a failable and asynchronous use case (the error is always the `Left` type of `Either`)

We have identified the following use cases for the user management subdomain:
* `GetAuthState`: provides the `AuthState` (whether an user is currently logged in or not);
* `GetUser`: provides information about the currently logged in user;
* `SignUpUser`: signs up a new user given email, name and password;
* `LoginUser`: logs in a user given user credentialn;
* `ChangeUserPassword`: changes the current password of the user with a different one;
* `LogoutUser`: logs out the currently logged in user;
* `DeleteUser`: deletes the logged in user.

Some of the above use cases use the following auxiliary use cases for data validation:
* `ValidateUserName`: checks that the user name is valid;
* `ValidateUserEmail`: checks that the user email is valid according to [RFC822](https://www.ietf.org/rfc/rfc0822.txt?number=822);
* `ValidateUserPassword`: checks that the user password is valid.

Instead, for the parking slot management subdomain we have identified the following use cases:
* `FindParkingSlots`: finds all the parking slots within a certain radius from a position (`GeoPosition`);
* `ViewParkingSlot`: provides information about a single parking slot identified by a given id;
* `ViewCurrentParkingSlot`: provides information about the parking slot currently occupied by the user;
* `OccupyParkingSlot`: occupies a parking slot if it is not currently occupied;
* `IncrementParkingSlotOccupation`: increments the occupation of the parking slot currently occupied by the user;
* `FreeParkingSlot`: frees the parking slot currently occupied by the user, if the user isn't occupying a parking slot it does nothing.

