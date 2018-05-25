# How to better organize your React applications?

Here is the source: https://medium.com/@alexmngn/how-to-better-organize-your-react-applications-2fd3ea1920f1

> `__*Each component, scene or service (a feature) has everything it needs to work on its own*__`, such as its own styles, images, translations, set of actions as well as unit or integration tests. You can see a feature like an independent piece of code you will use in your app (a bit like node modules).

To work properly, they should follow these rules:

* A component can define nested components or services. It cannot use or define scenes.
* A scene can define nested components, scenes or services.
* A service can define nested services. It cannot use or define components or scenes.
* Nested features can only use from its parent.

*Note*: By parent feature, I mean a parent, grandparent, great-grandparent etc… **You cannot use a feature that is a “cousin”, this is not allowed.** You will need to move it to a parent to use it.

## 1. Components
Components defined at the root level of your project, in the components folder, are global and can be used anywhere in your application. But if you decide to define a new component inside another component (nesting), this new component can only be used its direct parent.

Example:
````
   /src
     /components
       /Button
         /index.js
       /Notifications 
         /components 
           /ButtonDismiss 
             /index.js
         /actions.js
         /index.js
         /reducer.js
````

* __*Button*__ can be used anywhere in your application.
* __*Notifications*__ can also be used anywhere. 
    This component defines a component __*ButtonDismiss*__.
    You `CANNOT` use __*ButtonDismiss*__ anywhere else than in the __*Notifications*__ component.
* __*ButtonDismiss*__ uses Button internally, this is authorized because __*Button*__ is defined at the root level of components.

## 2. Sences
A scene is a page of your application. 
With the same principle components can be nested, you can also nest a scene into a scene, and also define components or services into a scene. You have to remember that if you decide to define something into a scene, you can only use it within the scene folder itself.

Example:
````
/src
  /scenes
    /Home 
      /components
        /ButtonShare
          /index.js
      /index.js
    /Sign
      /components
        /ButtonHelp
          /index.js
      /scenes
        /Login
          /components 
            /Form
              /index.js
            /ButtonFacebookLogin
              /index.js
          /index.js
       
        /Register
          /index.js
      /index.js
````
* __*Home*__ has a component __*ButtonShare*__, it can only be used by the __*Home*__ scene.
* __*Sign*__ has a component __*ButtonHelp*__. This component can be used by __*Login*__ or __*Register*__ scenes, or by any components defined in those scenes.
* __*Form*__ component uses __*ButtonHelp*__ internally, this is authorized because __*ButtonHelp*__ is defined by a parent.
* The __*Register*__ scene cannot use any of the components defined in __*Login*__, but it can use the __*ButtonHelp*__.

## 3. Services
Not everything can be a component, and you will need to create independent modules that can be used by your components or scenes.

You can see a service like a self-contained module where you will define the core business logic of your application. This can eventually be shared between several scenes or even applications, such as a web and native version of you app.

````
/src
  /services
    /api
      /services
        /handleError
          /index.js
      /index.js
    /geolocation 
    /session 
      /actions.js
      /index.js
      /reducer.js
````

I recommend you to create services to manage all api requests. You can see them as a bridge/an adapter between the server API and the view layer (scenes and components) of your application. It can take care of network calls your app will make, get and post content, and transform payloads as needed before being sent or saved in the store of your app (such as Redux). The scenes and components will only dispatch actions, read the store and update themselves based on the new changes.

___
