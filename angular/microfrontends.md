Sure! Here's a **complete example** of how to implement **Micro-Frontend Architecture using Angular with Webpack Module Federation**.

---

### 🔧 What is Module Federation?

Module Federation is a Webpack 5 feature that allows you to **share code dynamically between different applications** at runtime. It enables **microfrontends**, where different parts of your app can be independently developed, deployed, and run.

---

## 🎯 Goal

We will create:

* **Shell app** – the main host application.
* **Remote app** – a microfrontend that exposes an Angular module/component.

These apps will be **integrated using Module Federation**.

---

## 📁 Project Structure

```
microfrontends/
├── shell/           → Host application
└── mfe1/            → Remote application (Microfrontend)
```

---

## 1️⃣ Set Up the Shell (Host) App

```bash
ng new shell --routing --style css
cd shell
ng add @angular-architects/module-federation --project shell
```

When prompted:

* Select "Host"
* Provide port: `4200`

This will modify:

* `webpack.config.js`
* `webpack.prod.config.js`
* `bootstrap.ts`
* `main.ts`

---

## 2️⃣ Set Up the Remote (Microfrontend) App

```bash
cd ..
ng new mfe1 --routing --style css
cd mfe1
ng add @angular-architects/module-federation --project mfe1
```

When prompted:

* Select "Remote"
* Provide port: `4201`
* Name: `mfe1`

---

## 3️⃣ Expose a Component or Module from `mfe1`

In `mfe1/projects/mfe1/src/app/app.module.ts`:

Create a new component:

```bash
ng generate component hello
```

Then in `webpack.config.js`, expose it:

```js
// mfe1/webpack.config.js
const { withModuleFederation } = require('@angular-architects/module-federation/webpack');

module.exports = withModuleFederation({
  name: 'mfe1',

  exposes: {
    './HelloModule': './src/app/hello/hello.module.ts',
  },

  shared: {
    "@angular/core": { singleton: true, strictVersion: true },
    "@angular/common": { singleton: true, strictVersion: true },
    "@angular/router": { singleton: true, strictVersion: true },
  },
});
```

Create a module for the component:

```ts
// mfe1/src/app/hello/hello.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { HelloComponent } from './hello.component';

@NgModule({
  declarations: [HelloComponent],
  imports: [CommonModule],
  exports: [HelloComponent],
})
export class HelloModule {}
```

---

## 4️⃣ Load Remote Module in Shell

In the `shell` app, update `webpack.config.js` to add `mfe1` as a remote:

```js
// shell/webpack.config.js
const { withModuleFederation } = require('@angular-architects/module-federation/webpack');

module.exports = withModuleFederation({
  remotes: {
    mfe1: 'mfe1@http://localhost:4201/remoteEntry.js',
  },

  shared: {
    "@angular/core": { singleton: true, strictVersion: true },
    "@angular/common": { singleton: true, strictVersion: true },
    "@angular/router": { singleton: true, strictVersion: true },
  },
});
```

Then, lazy load the remote module using `loadRemoteModule`:

```ts
// shell/src/app/app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { loadRemoteModule } from '@angular-architects/module-federation';

const routes: Routes = [
  {
    path: 'hello',
    loadChildren: () =>
      loadRemoteModule({
        type: 'module',
        remoteEntry: 'http://localhost:4201/remoteEntry.js',
        exposedModule: './HelloModule',
      }).then(m => m.HelloModule),
  },
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule],
})
export class AppRoutingModule {}
```

---

## 5️⃣ Run the Apps

In separate terminals:

```bash
# Terminal 1
cd shell
ng serve

# Terminal 2
cd mfe1
ng serve
```

Now navigate to `http://localhost:4200/hello` – you'll see the remote component (`HelloComponent` from `mfe1`) rendered inside the shell!

---

## ✅ Summary

| App   | Port | Role         | Description                                |
| ----- | ---- | ------------ | ------------------------------------------ |
| Shell | 4200 | Host         | Loads microfrontends via Module Federation |
| MFE1  | 4201 | Remote (MFE) | Exposes a module/component                 |

---

## 🧠 Tips

* Use **Version Mismatch Warnings**: `strictVersion: true` helps avoid dependency conflicts.
* For **shared state**, consider libraries like **RxJS**, or **NgRx**.
* Deploy each MFE independently; they’ll update in the host automatically.

---

Would you like me to generate the full code for both projects or add authentication/routing integration?
