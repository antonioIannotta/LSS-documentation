---
title: Design
has_children: false
nav_order: 5
---

# Design

## Architecture and implementation
The system is composed of a frontend and a backend. The Android app serves as the frontend, responsible for managing user interactions, while the backend ensures consistent parking slot data and managing sign-up/login procedures. \
This separation between the frontend and the backend allows us to swap either component at any time without affecting the other. Moreover, it permits to introduce new clients, written in different programming languages, such as an iOS app developed using Swift. To fully understand the architecture of our system, it is necessary to take a closer look at both the frontend and backend.

### Backend
The backend is composed of the database and the software that is in charge to define the logic behind the access to the database. More specifically, there are two main components:
* **Database** two different databases are used for the application, one for the user authentication bounded context and one for the parking slot management bounded context. Both databases use MongoDB Atlas.
* **Web Service**
While there's not a lot to say about the database it's important to talk about the web service. It exposes two main routes, **/user** and **/parking-slot**: each of these routes is in charge to recall logic to handle either the user access/registration to the service or to retrieve/update the infromation related to the parking slots from the databases.

#### Backend: general concepts
There have been several theoretical considerations that led to choosing a specific typology of either database or web framework rather than another one.
* **Database**: the choice of a NoSQL database has been the result of multiple considerations concerning scalability, performance, and geographical query support. The possibility to scale horizontally splitting parking slot data into different shards would be really good also for the availability. Also, geographical queries are fast on NoSql databases and *MongoDB* offers first-party support to them. Furthermore, MongoDB offers a Cloud solution named *MongoDB Atlas* that can be easily swapped at any moment with its open-source counterpart.
* **Web Service**: the choice of the web framework to use for the backend is led by the target of having a single language across the whole application. To achieve this goal we choose Ktor as the web framework (implemented in Kotlin). To implement the routes for the web service we will adopt a ReST approach.

#### Backend: architecture
The architecture of the backend follows the rules of the **Clean architecture**. This architecture lead to the detection of several layers for the application and is applied to both the two modules that the backend has: **user** and **parking slot**. The layers are the following:
* **entity**
* **use cases**
* **interface adapter**
* **framework**

In the following we report the architecture for each module:

##### User 
* Entity:
    - **User**: this entity contains information about the user: email, password and name.
    - **User credentials**: represents an email-password pair used by the user to log in to the system.
    - **User info**: this entity represents the user's not sensitive information. It corresponds to a user entity without the password field.
* Use cases:
    - **Login**: a user provides his email and password to access the system.
    - **Create user**: a user wants to be registered on the sistem.
    - **Get info**: a user wants to see their account information.
    - **Change password**: a user wants to change their password with a new one. 
    - **Delete user**: a registered user wants to delete their account.
    - **Validate**: an email-password pair must be validated (checking if a user with this email exists in the database and if the associated password corresponds to the one provided).
    - **Exists**: an user email must be verified checking in the database if there's a registered user with this email

* Interface adapter:
    - handles the framework layer requests, translating them into use cases.

* Framework:
    - It's represented by KTOR web server and Mongo database.

##### Parking slot
* Entity:
    - **Paring Slot**: this entity is completely mapped into the database and represents a single parking slot. Any other component of the parking slot module depends on the parking slot entity.

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
* **use_cases** package contains an interface that describe the behaviour of all the use cases.
* **interface_adapter** package contains a class UserInterfaceAdapter that implements the use cases. It also includes two submodules: a model package and a utils package. The model subpackage contains data classes that define the request/response body structure that should be used by the framework layer. The utils subpackage contains utility functions such as email sender method and jwt handler methods (generation and validation).

##### Parking Slot
In order to talk about the implementation of the parking slot operations inside the submodule **parking-slot** it is important to keep together implementation and clean architecture. The clean architecture previously presented is composed of four layers. The three inner layers (entity, use cases, and interface adapter) correspond to three packages in the **parking-slot** submodule. The fourth layer (framework) is outside this module by the moment it is common to both **parking-slot** and **user** submodules. We now report more specifically the elements that fill any of the three layers of the **parking-slot** submodule:
* **entity**: in this package, there are all those entities that are the domain basis for the parking-slot subproject. These core entities are:
  * **Position**: represents a single position expressed as latitude-longitude coordinates. This entity is helpful because it provides the exact location for each parking slot.
   * **Center**: represents the area around which the user is interested in the status of the parking slots.
   * **StopEnd**: represents the end stop set or incremented for a certain parking slot.
   * **ParkingSlot**: represents a single parking slot with its properties: the status (either occupied or not), the position expressed as latitude and longitude, the end stop time, and the identifier (that is unique for each parking slot)
 All these entities are modeled as **Kotlin** classes. This choice is derived from the consideration of the possible inheritance of these classes. By the moment these entities could be subjected to inheritance in future uses and future development of the domain the choice has been to model them as classes.

* **use_cases**: in this package are reported the use cases, representing the second layer of the clean architecture previously presented. Remaining on the implementation side the use cases have been represented as methods of an interface to be implemented by the interface adapter. This is because the interface adapted is in charge to implement the methods translating into it the framework requests.

* **interface_adapter**: this package contains a class, **InterfaceAdapter**, that receives as a constructor parameter a collection of the MongoDB database, in this case. This class implements the UseCases interface and provides the results that are used in the framework package to provide the responses to the incoming requests from the client. It's important to notice how implementing the Interface adapter in this way, the use cases remain independent from it and, in case of a change of the Database, will only be necessary to change the constructor argument and some operations inside this class. This solution has been thought to be aligned with the SoC principle and, furthermore, to be coherent with the previous domain analysis.

### Frontend
The frontend follows the Clean Architecture pattern. Clean Architecture was first introduced by Robert C. Martin in his book "Clean Architecture: A Craftsman's Guide to Software Structure and Design". It is a way of organizing a software system in a way that separates the concerns of the various components, making it easier to understand and maintain. Specifically, we obtain the following advantages:
* **modularity**: it helps to create a clear separation of concerns within the codebase. Each layer is decoupled from the others and has a specific purpose;
* **testability**: since the inner circle is independent of the outer layers, it is way easier to write unit tests that focus on the business rules:
* **loose coupling**: it's easier to swap out external dependencies without affecting the business logic;
* **flexibility**: the separation of concerns Clean Architecture provides makes it easier to modify and adapt the code to changing requirements;
at expenses of:
  * **boilerplate**: achieving indirection requires an important number of interfaces;
  * **learning curve**: initially it is relatively difficult to learn differently from other patterns.
Finally, the choice is all about the current size of the project and how much we expect it to grow.

Following this architecture we organized the frontend in four layers:
* The **domain layer** contains all the entities, use cases and repositories. It defines the core concepts and business rules of the app and use cases. The domain layer does not depend on any other layer.
* The **data layer** (also called *infrastructure layer*)is responsible for managing the interaction with local storage, backend API and system services. It contains data sources and repositories implementation. This layer only depends on the domain layer.
* The **presentation layer** contains the user interface components. This module only depends on the domain layer.
* The **app layer** depends on all three previous layers and it is responsible for the launch of the app.

By separating the frontend into distinct layers, we can ensure that the different parts of the app are decoupled and can be developed and tested independently. Both the data layer and the presentation layer only depend on interfaces (Dependency Inversion Principle). To give the proper implementation to the layers that need it we will use the service locator pattern.

_In the following UML Class Diagrams it is possible to find the `async` keyword: it means that the method does not return immediately: it is asynchronous._

#### Domain Layer
The domain layer is furtherly split down into `user` and `parking slot` packages to isolate the DDD subdomains.

##### Entities

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/design/entities_parking_slot.png?raw=true"/>
  <figcaption>Parking slot package entities</figcaption>
</figure>
<br/>

In the above UML Class Diagram there are multiple entities used to represent the parking slot, which is identified by an `id` and contains information about its position and its state (whether it is occupied or not).

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/design/entities_user.png?raw=true"/>
  <figcaption>User package entities</figcaption>
</figure>
<br/>

In the above UML Class Diagram there are several classes used to represent the user. Specifically:

* **User**: a user registered in the system;
* **NewUser**: a new user to be added in the system;
* **UserCredentials**: the credentials to be used to log in to the system;
* **AuthState**: whether an user is currently logged in or not.

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
  <figcaption>Basic use cases</figcaption>
</figure>
<br/>

To simplify the creation of use cases we have created some abstract classes which all the use cases inherit from.
* `UseCase`: a non-failable use case that always returns in a non-asynchronous manner
* `AsyncUseCase`: a non-failable asynchronous use case
* `AsyncFailableUseCase`: a failable and asynchronous use case (the error is always the `Left` type of `Either`)

We have identified the following use cases for the user management subdomain:
* `GetAuthState`: provides the `AuthState` (whether an user is currently logged in or not);
* `GetUser`: provides information about the currently logged-in user;
* `SignUpUser`: signs up a new user given email, name and password;
* `LoginUser`: logs in a user given user credentials;
* `ChangeUserPassword`: changes the current password of the user to a different one;
* `LogoutUser`: logs out the currently logged-in user;
* `DeleteUser`: deletes the logged-in user.

Some of the above use cases use the following auxiliary use cases for data validation:
* `ValidateUserName`: checks that the user name is valid;
* `ValidateUserEmail`: checks that the user email is valid according to [RFC822](https://www.ietf.org/rfc/rfc0822.txt?number=822);
* `ValidateUserPassword`: checks that the user password is valid.

As for the parking slot management subdomain, we have identified the following use cases:
* `FindParkingSlots`: finds all the parking slots within a certain radius from a position (`GeoPosition`);
* `ViewParkingSlot`: provides information about a single parking slot identified by a given id;
* `ViewCurrentParkingSlot`: provides information about the parking slot which is currently occupied by the user;
* `OccupyParkingSlot`: occupies a parking slot if it is not currently occupied;
* `IncrementParkingSlotOccupation`: increments the occupation of the parking slot which is currently occupied by the user;
* `FreeParkingSlot`: frees the parking slot which is currently occupied by the user; if the user isn't occupying a parking slot it does nothing.

#### Data Layer
This layer contains the implementation of the repositories and data sources. In our system, repositories act as mediators between different data sources, such as local authentication token storage and backend API.
This allows for a better decoupling and testability, since repository implementation does not need to access the APIs exposed by the underlying system, unlike the data sources.
To further decouple the domain layer from the data layer, we make data sources return data transfer objects (DTOs). DTOs are simple objects that only contain the data needed to read information from the backend. These objects are then mapped by the repositories into domain layer entities. 

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/design/datasources.png?raw=true"/>
  <figcaption>Data sources UML Class Diagram</figcaption>
</figure>
<br/>

In the above class diagram we can see all the data sources used by repositories (for brevity not all DTOs are shown). The fact that we are looking at classes which are meant to interact with the backend (`ParkingSlotDataSource`, `UserDataSource`) is made clear by the `Body` suffix in some method arguments, which are the bodies of the requests to submit to the backend. The usage of data sources and DTOs allows us to hide these details. 

#### Presentation Layer
This layer contains all the user interface components, and, like the other layers, is splitted into `user` and `parking slot` packages. 

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/design/mvvm.png?raw=true"/>
  <figcaption>MVVM</figcaption>
</figure>
<br/>

Here, we decide to use the MVVM design pattern. MVVM separates the concerns of the UI presentation and the business logic:
* the view is responsible for displaying the UI;
* the model encapsulate the app's data;
* the view-model acts as the intermediary between the view and the model. This separation makes the code easier to maintain and test. Furthermore, it improves testability by allowing us to test the business logic without having to test the UI components. That means there is no need for simulators, emulators or real devices to test the business logic.

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/design/presentation_router.png?raw=true"/>
  <figcaption>Router</figcaption>
</figure>
<br/>

In the UML Class Diagram above the `Router` is shown: it exposes two methods for navigating to a specific screen and dismissing the current screen. Moreover, there is an observable containing the navigation requests ( `RouterCommand` ). This observable is used by the system to know when to navigate to a specific screen.

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/design/presentation_app_alert.png?raw=true"/>
  <figcaption>App Alert</figcaption>
</figure>
<br/>

##### Parking Slot

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/design/presentation_parking_slot.png?raw=true"/>
  <figcaption>Parking Slot Management Subdomain Presentation Layer UML Class Diagram</figcaption>
</figure>
<br/>

The `parkingslot` package contains:
* **Parking Slots Screen**: a screen with a map showing the nearby area with all the parking slots; 
* **Parking Slot Screen**: a screen that shows all the information about a specific parking slot.

##### User

<figure align="center">
  <img src="https://github.com/antonioiannotta/LSS-documentation/blob/main/design/presentation_user.png?raw=true"/>
  <figcaption>User Management Subdomain Presentation Layer UML Class Diagram</figcaption>
</figure>
<br/>

The `user` package contains:
* **Login Screen**: the screen that allows the user to log in; 
* **Sign Up Screen**: the screen that allows a new user to sign up;
* **Change Password Screen**: the screen that allows the user to change the password.
