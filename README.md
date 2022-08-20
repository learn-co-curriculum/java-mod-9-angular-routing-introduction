# Routing Introduction

## Learning Goals

- Describe routing.

## Routing

As a reminder, let's have another look at the hierarchy of our components in our
messaging application:

![Messaging Application Component Hierarchy](https://curriculum-content.s3.amazonaws.com/java-mod-8/ng-messaging-component-hierarchy.png)

As you will remember, we have 2 level 1 components, the "header" and the
"application panel". In the application panel, where we display various sections
of the application, we currently have 3 level 2 components, the "conversation
control", the "conversation thread" and the "contact list". The "contact list"
component is currently shown at the same time as the other components, which is
what we're going to address with routing.

Routing in a Single Page Application (SPA) is key because it allows the user
interface to change to reflect the end user's action without actually asking the
browser to reload the entire application.

Another way to think about routing is "navigation" and the ability to go to
different places in the application is generally something that is easy to
understand as being needed all over the application. Thinking about this way
might help you understand why the first step in setting up routes is to add them
to our root module in `app.module.ts`:

- First, we need to import Angular's `Router`, which is the type of the object
   we will need to set up our routes, and Angular's `RouterModule`, which is the
   module that will be responsible for managing the routes we define:

```typescript
import { RouterModule, Routes } from "@angular/router";
```

- Then we need to add a `const` of type `Routes` to our app module to define
   the routes we want to support. Each route takes 2 parameters:
   1. The `path`, which is the path we want the route to be activated for. This
      should not have the leading "/" in it, so a path for
      `http://localhost:4200/contactList` for example would be set up as
      `contactList`.
   2. The `component`, which is the name of the component that should be shown
      on the UI when that route is activated

```typescript
const appRoutes: Routes = [
  { path: "", component: ApplicationComponentComponent },
  { path: "contactList", component: ContactListComponentComponent },
];
```

- Then we need to add Angular's router module to the list of imports in the app
   module:
   1. Here we call the `forRoot()` function in the router module to give it the
      routes that we defined earlier

```typescript
  imports: [
    BrowserModule,
    FormsModule,
    HttpClientModule,
    RouterModule.forRoot(appRoutes)
  ],
```

At this point, our app module is aware of Angular's router module, and the
router module has been initialized with our routes. The only thing that's left
to do is tell our views where we actually want each route's component to be
displayed.

So in the `app.component.html`, instead of always showing the
`app-application-component` like we were before:

```html
<div class="container">
  <div class="row">
    <div class="col-12 border p-3">
      <app-header-component></app-header-component>
    </div>
  </div>
</div>

<div class="container">
  <div class="row">
    <div class="col-12 border p-3">
      <app-application-component></app-application-component>
    </div>
  </div>
</div>
```

We are going to show whatever component the router tells us should be shown:

```html
<div class="container">
  <div class="row">
    <div class="col-12 border p-3">
      <app-header-component></app-header-component>
    </div>
  </div>
</div>

<div class="container">
  <div class="row">
    <div class="col-12 border p-3">
      <router-outlet></router-outlet>
      <!-- placing the active component based on the route here -->
    </div>
  </div>
</div>
```

Note that we are leaving the `<app-header-component></app-header-component>`
where it is, which is to say we want the header to stay visible across all our
routes, and only replace the area below it with whatever component the route
tells us to display.

With that, our `app.component.html` now looks like this:

```html
<div class="container">
  <div class="row">
    <div class="col-12 border p-3">
      <app-header-component></app-header-component>
    </div>
  </div>
</div>

<div class="container">
  <div class="row">
    <div class="col-12 border p-3">
      <router-outlet></router-outlet>
    </div>
  </div>
</div>
```

One last touch: we need to remove the
`<app-contact-list-component></app-contact-list-component>` from our
`application-component.component.html` view - this is because the application
component is no longer responsible for displaying the contact list component -
the router will now handle that when the right route is loaded:

```html
<div class="container">
  <div class="row">
    <div class="col-12 p-3">
      <app-conversation-control-component></app-conversation-control-component>
    </div>
  </div>
</div>

<div class="container">
  <div class="row">
    <div class="col-12 border p-3">
      <app-conversation-history-component></app-conversation-history-component>
    </div>
  </div>
</div>

<!-- remove this entire section -->
<div class="container">
  <div class="row">
    <div class="col-12 border p-3">
      <app-contact-list-component></app-contact-list-component>
    </div>
  </div>
</div>
<!-- end of section to remove -->
```

You should now be able to load each route and only see the header and the
component for that route:

1. The default route: ![Default Route](https://curriculum-content.s3.amazonaws.com/java-mod-8/ng-messaging-default-route.png)
2. The `contactList` route: ![](https://curriculum-content.s3.amazonaws.com/java-mod-8/ng-messaging-contact-list-route.png)
