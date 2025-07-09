
## 🧪 Pure vs Impure Pipes in Angular

### ✅ **Pure Pipes** (default behavior)

> Angular only calls pure pipes **when the input reference changes**.

```ts
@Pipe({
  name: "myPurePipe",
  pure: true, // This is the default
})
export class MyPurePipe implements PipeTransform {
  transform(value: any): any {
    // transform logic
  }
}
```

#### 🔹 How It Works:

- Called **only** when Angular detects a change in the input value **by reference** (using `===`).
- No re-evaluation if internal properties change (e.g., a new item added to the same array object).

#### 🔸 Pros:

- Super performant.
- Ideal for **stateless transforms** (e.g., formatting numbers, strings, enums).

#### 🔸 Cons:

- Doesn’t detect **mutation** within objects or arrays.
- You must replace the whole object (immutability) to trigger a re-evaluation.

---

### ❗ **Impure Pipes**

> Angular calls impure pipes **on every change detection cycle**, regardless of input reference.

```ts
@Pipe({
  name: "myImpurePipe",
  pure: false,
})
export class MyImpurePipe implements PipeTransform {
  transform(value: any): any {
    // transform logic
  }
}
```

#### 🔹 How It Works:

- Evaluated **every time** Angular runs change detection — even if the input hasn't changed.
- Angular treats it like it could return a different output every time.

#### 🔸 Pros:

- Useful for things that **constantly change**, like:
  - Filtering or sorting arrays **without changing the reference**
  - Fetching data dynamically
  - Pipes that depend on **global state** (e.g., current time, translations)

#### 🔸 Cons:

- **Very expensive** in large templates or complex UIs.
- Should be avoided unless necessary.

---