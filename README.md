#How to better organize your React applications?

Here is the source: https://medium.com/@alexmngn/how-to-better-organize-your-react-applications-2fd3ea1920f1

__*Each component, scene or service (a feature) has everything it needs to work on its own*__, such as its own styles, images, translations, set of actions as well as unit or integration tests. You can see a feature like an independent piece of code you will use in your app (a bit like node modules).

To work properly, they should follow these rules:

* A component can define nested components or services. It cannot use or define scenes.
* A scene can define nested components, scenes or services.
* A service can define nested services. It cannot use or define components or scenes.
* Nested features can only use from its parent.

*Note*: By parent feature, I mean a parent, grandparent, great-grandparent etc… **You cannot use a feature that is a “cousin”, this is not allowed.** You will need to move it to a parent to use it.

####Components
Components defined at the root level of your project, in the components folder, are global and can be used anywhere in your application. But if you decide to define a new component inside another component (nesting), this new component can only be used its direct parent.

__Example:__

>   /src
>     /components
>       /Button
>         /index.js
>       /Notifications 
>         /components 
>           /ButtonDismiss 
>             /index.js
>         /actions.js
>         /index.js
>         /reducer.js

* __*Button*__ can be used anywhere in your application.
* __*Notifications*__ can also be used anywhere. 
    This component defines a component __*ButtonDismiss*__.
    You **CANNOT** use __*ButtonDismiss*__ anywhere else than in the __*Notifications*__ component.
* __*ButtonDismiss*__ uses Button internally, this is authorized because __*Button*__ is defined at the root level of components.
