---
title: Implementation
has_children: false
nav_order: 7
---

# Implementation

## Backend

The technologies used for the implementation of the backend are the following:
* [Ktor](https://ktor.io/): this is the web framework of the Kotlin language used to implement the Rest API.
* [MongoDB](https://www.mongodb.com/): this is the NoSQL databse choose for the purpose of this application.
* [Gradle](https://gradle.org/): as build automation tool has been chosen Gradle.

In the previous section has been reported as the application is composed of two modules related to the functions for the **user** and the functions for the **parking slot**.\ 
The user operations are all those operations related to the access of the user to the system. The parking slot operations are all those operations that are related to the management of parking slot status. These operations are mapped into two submodules for the implementation side correspond to two several collections into the database.
Considering the implementation aspects each module is a single gradle module. More specifically there are three modules:
* **Parking system**: this is the main module, in which there's the access point for the application and the framework layer from the previous paragraph. 
    - **user**: the submodule related to the operations for the user access to the system.
    - **parking-slot**: the submodule related to the operations for the parking-slot management.


## Frontend
Here we will discuss the implementation details of the frontend. The frontend uses a great number of libraries and frameworks among which we can find:
* [Jetpack Compose](https://developer.android.com/jetpack/compose);
* [Koin](https://insert-koin.io);
* [KotlinX Serialization](https://github.com/Kotlin/kotlinx.serialization);
* [KotlinX DateTime](https://github.com/Kotlin/kotlinx-datetime);
* [Retrofit](https://square.github.io/retrofit/);
* [Google Maps SDK](https://developers.google.com/maps/documentation/android-sdk/maps-compose).
In the following paragraphs, we will try to present each one of these libraries and say why we think each one is a good fit for the app.

### Jetpack Compose
Jetpack Compose was chosen as the UI toolkit for building the app. It allows developers to get rid of XML layout files and use newer, easier-to-test declarative UIs. XML layouts forced us to use a lot of different files, and deal with lower-level stuff like view id. Moreover, even if it's pretty young it is greatly integrated with the Android ecosystem and already offers wrappers for all the common functionality needed. Finally, even if it's not needed for this project, it provides multiplatform support, so it can be deployed also to Web and Desktop platforms.

```kotlin
Column(
    modifier = Modifier
        .padding(32.dp)
        .fillMaxHeight(),
    verticalArrangement = Arrangement.spacedBy(
        10.dp,
        alignment = Alignment.CenterVertically
    )
) {
    Text(
        text = stringResource(R.string.screen_title_login),
        fontSize = 40.sp,
    )

    EmailTextField(
        uiState = uiState,
        onEmailChange = onEmailChange
    )
    PasswordTextField(
        uiState = uiState,
        onPasswordChange = onPasswordChange
    )
    SubmitButton(
        uiState = uiState,
        onSubmit = onSubmit
    )

    SignUpButton(
        uiState = uiState,
        onSignUpClick = onSignUpClick
    )
}
```
<p align="center">Jetpack Compose - Login Form</p>

### Koin
In the design chapter, we decided to use the service locator pattern. A key enabler of this pattern is a dependency injection framework: service-locator pattern can be pretty risky: if you forget to declare a dependency the app will crash at runtime. To avoid that, Koin provides a lot of utilities to test that all the dependencies are registered in tests to avoid bugs sneaking into production.

```kotlin
koinApplication {
    modules(domainModule)
    modules(presentationModule)
    checkModules {
        withInstance<UserRepository>(mockk())
        withInstance<ParkingSlotRepository>(mockk())
    }
}
```
<p align="center">Koin Dependency Testing</p>

### KotlinX Serialization
Kotlin Serialization is a serialization library that allows developers to serialize Kotlin data classes to and from JSON. It is enough to annotate Kotlin data classes as `Serializable` to make them readable from JSON, this, paired with the usage of DTOs allowed us to write very little code for it.


### KotlinX DateTime
kotlinx-datetime was chosen as the date and time library for the Android app. We chose this library because, like Koin, Kotlin Serialization, and Jetpack Compose, it offers multiplatform support and also is deeply integrated with the Kotlin ecosystem and so we don't have to write serializers and deserializers for dates.

### Retrofit
Retrofit was chosen as the networking library for the Android app. It is a networking library for Android that simplifies the process of making HTTP requests and handling responses. It allows us to write data sources easily since we only have to annotate the fields in the methods.

```kotlin
@GET("parking-slot/")
suspend fun getParkingSlots(
    @Query("latitude")
    latitude: Double,
    @Query("longitude")
    longitude: Double,
    @Query("radius")
    radius: Double,
): List<ParkingSlotDto>
```
<p align="center">Retrofit - getParkingSlots method</p>

