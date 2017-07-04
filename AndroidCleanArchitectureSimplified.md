From [A detailed guide on developing Android apps using the Clean Architecture pattern](https://medium.com/@dmilicic/a-detailed-guide-on-developing-android-apps-using-the-clean-architecture-pattern-d38d71e94029)


# Structure
The general structure for an Android app looks like this:
+ **Outer layer packages**: UI, Storage, Network, etc.
+ **Middle layer packages**: Presenters, Converters
+ **Inner layer packages**: Interactors, Models, Repositories, Executor

## Outer layer
As already mentioned, this is where the framework details go.
+ **UI**  — This is where you put all your Activities, Fragments, Adapters and other Android code related to the user interface.
+ **Storage**  — Database specific code that implements the interface our Interactors use for accessing data and storing data. This includes, for example, ContentProviders or ORM-s such as DBFlow.
+ **Network**  — Things like Retrofit go here.

## Middle layer
Glue code layer which connects the implementation details with your business logic.
+ **Presenters**  — Presenters handle events from the UI (e.g. user click) and usually serve as callbacks from inner layers (Interactors).
+ **Converters**  — Converter objects are responsible for converting inner models to outer models and vice versa.

## Inner layer
The core layer contains the most high-level code. All classes here are POJOs. Classes and objects in this layer have no knowledge that they are run in an Android app and can easily be ported to any machine running JVM.
+ **Interactors**  — These are the classes which actually contain your business logic code. These are run in the background and communicate events to the upper layer using callbacks. They are also called UseCases in some projects (probably a better name). It is normal to have a lot of small Interactor classes in your projects that solve specific problems. This conforms to the Single Responsibility Principle and in my opinion is easier on the brain.
+ **Models**  — These are your business models that you manipulate in your business logic.
+ **Repositories**  — This package only contains interfaces that the database or some other outer layer implements. These interfaces are used by Interactors to access and store data. This is also called a repository pattern.
+ **Executor**  — This package contains code for making Interactors run in the background by using a worker thread executor. This package is generally not something you need to change.
