

 In Angular, the modules can be loaded:

- Eager (when the application starts)
- Lazy (when needed)

const routes: Routes = [
  // Regular routes
  { path: "home", component: HomeComponent },
  { path: "about", component: AboutComponent },

  // Lazy-loaded module
  {
    path: "lazy",
    loadChildren: () => import("./lazy/lazy.module").then((m) => m.LazyModule),
  },
];

- Preloaded
The Angular router loads modules asynchronously in the background after the main application has been loaded



 Router Guards
- Allow you to execute logic before or after navigating to a route 
- Router guards are crucial for implementing features like authentication, authorization, and data fetching in Angular applications.