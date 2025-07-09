## Jit & AOT

In Angular, **JIT (Just-in-Time)** and **AOT (Ahead-of-Time)** are two types of **compilation strategies** used to convert Angular HTML and TypeScript code into efficient JavaScript that can run in the browser.

---

### 🏗️ What Is Compilation in Angular?

Angular applications are written in **HTML templates + TypeScript**, which the browser can't directly understand. So Angular compiles them into **JavaScript** using either:

* **JIT (Just-in-Time) compilation** — happens in the **browser at runtime**
* **AOT (Ahead-of-Time) compilation** — happens during the **build time before deployment**

---

### ⚡ JIT (Just-in-Time) Compilation

* 🕒 **When**: Compilation happens in the **browser** when the app is loaded.
* 🔧 **Use case**: Commonly used during **development** because of faster build times and easier debugging.
* ⚠️ **Disadvantages**:

  * Slower initial load (compilation happens at runtime)
  * Larger bundle size (includes compiler code)
  * Less secure (source code is more exposed)

#### ✅ Pros:

* Fast rebuilds during development
* Easier to debug due to more descriptive error messages

The Angular application is compiled every time it is loaded.

---

### 🚀 AOT (Ahead-of-Time) Compilation

* 🏗️ **When**: Compilation happens during the **build process** (e.g., with `ng build --prod`)
* 📦 **Use case**: Used in **production** for optimized performance and security.
* 💡 Angular CLI uses AOT by default in production mode.

#### ✅ Pros:

* Faster app load time (templates already compiled)
* Smaller bundle size (no Angular compiler needed at runtime)
* Early error detection (compilation errors are caught during build)
* Improved security (no template compilation in the browser)

This means the browser receives the pre-compiled JavaScript code, ready to be executed immediately.

How AOT Works
---

### 🆚 Summary Table

| Feature          | JIT                        | AOT                             |
| ---------------- | -------------------------- | ------------------------------- |
| Compilation Time | Runtime (in browser)       | Build time (before deployment)  |
| Speed            | Slower initial load        | Faster load, better performance |
| Bundle Size      | Larger (includes compiler) | Smaller (compiled ahead)        |
| Error Detection  | Runtime                    | Compile-time                    |
| Use Case         | Development                | Production                      |

---

### 👨‍💻 How to Use in Angular CLI

* **JIT (default for `ng serve`)**:

  ```bash
  ng serve
  ```

* **AOT**:

  ```bash
  ng build --prod
  ```

Or explicitly:

```bash
ng build --aot
```

---

