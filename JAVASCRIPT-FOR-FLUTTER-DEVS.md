# JavaScript Guide for Flutter / Dart Developers

Tumhe **Dart + Flutter** aata hai. Yeh doc JavaScript ko usi language se compare karke sikhati hai.

Goal:
1. Pehle **JavaScript** strong karo  
2. Phir **Node.js + PostgreSQL** easy lagega  

Estimated path: **7–10 days** (roz 1–2 hours)

---

## Roadmap

| Day | Topic |
|-----|--------|
| 1 | Variables, types, Dart vs JS |
| 2 | Functions, arrow functions |
| 3 | Arrays & objects |
| 4 | Conditionals, loops |
| 5 | Async / await / Promises (bahut important for Node) |
| 6 | Modules (`import` / `require`) |
| 7 | Mini practice + Node readiness |

Har section ke baad **Tasks** hain — unhe khud solve karo.

---

# DAY 1 — Basics (Dart → JavaScript)

## 1.1 Variables

| Dart | JavaScript |
|------|------------|
| `var name = 'Ali';` | `var name = "Ali";` (purana — avoid) |
| `final name = 'Ali';` | `const name = "Ali";` |
| `var age = 20;` (reassign) | `let age = 20;` |

```js
const name = "Ali";     // change nahi hoga (jaise final)
let age = 20;           // change ho sakta hai
age = 21;

// const object/array ki andar values change ho sakti hain:
const user = { name: "Ali" };
user.name = "Ahmed";    // OK
// user = {};           // ERROR
```

**Rule:** pehle `const`, jab zarurat ho tab `let`. `var` mat use karo.

## 1.2 Types

JS mein types **runtime** pe decide hote hain (dynamic), Dart jaisa optional typing nahi (unless TypeScript).

```js
typeof "hello";   // "string"
typeof 10;        // "number"   (int/double alag nahi — sab number)
typeof true;      // "boolean"
typeof undefined; // "undefined"
typeof null;      // "object"   (JS quirk — yaad rakhna)
typeof {};        // "object"
typeof [];        // "object"
```

```js
let x;            // undefined
let y = null;     // intentionally empty
```

## 1.3 Strings

```js
const name = "Ali";
const msg = `Hello ${name}`;   // jaise Dart: 'Hello $name'
console.log(msg);

"hello".toUpperCase();
"hello".includes("ell");       // true
"a,b,c".split(",");            // ["a","b","c"]
```

## 1.4 Template vs Dart

| Dart | JS |
|------|-----|
| `'Hi $name'` | `` `Hi ${name}` `` |
| `'Sum ${a + b}'` | `` `Sum ${a + b}` `` |

## Tasks — Day 1

1. `const` aur `let` se 5 variables banao (name, age, city, isStudent, score)  
2. Template string se ek sentence print karo  
3. `typeof` se har variable ka type print karo  
4. Ek `const` object banao aur uski property change karke dekho  

---

# DAY 2 — Functions

## 2.1 Normal function

```js
function add(a, b) {
  return a + b;
}

console.log(add(2, 3)); // 5
```

## 2.2 Arrow function (Flutter/Dart ke lambda jaisa)

```dart
// Dart
int add(int a, int b) => a + b;
```

```js
// JavaScript
const add = (a, b) => a + b;
const greet = (name) => `Hello ${name}`;
const square = (n) => n * n;
```

Multi-line:

```js
const getUser = (name, age) => {
  return {
    name,
    age,
  };
};
```

## 2.2 Default + rest

```js
function greet(name = "Guest") {
  return `Hi ${name}`;
}

function sum(...nums) {
  // nums = array
  let total = 0;
  for (const n of nums) total += n;
  return total;
}

sum(1, 2, 3, 4); // 10
```

## 2.3 Callback (Flutter mein bhi aata hai)

```js
function doSomething(callback) {
  console.log("Working...");
  callback();
}

doSomething(() => {
  console.log("Done");
});
```

## Tasks — Day 2

1. Arrow function: `multiply(a, b)`  
2. Function: `isEven(n)` → true/false  
3. Function: `fullName(first, last)` → template string  
4. Callback practice: `runTwice(fn)` jo function ko 2 baar chalaye  

---

# DAY 3 — Arrays & Objects (Flutter lists/maps)

## 3.1 Arrays = Dart List

```js
const nums = [1, 2, 3];
nums.push(4);          // add end
nums.pop();            // remove end
nums.length;           // 3
nums[0];               // 1
```

### Important methods (Node APIs mein roz use)

```js
const users = [
  { id: 1, name: "Ali", age: 20 },
  { id: 2, name: "Sara", age: 25 },
  { id: 3, name: "Omar", age: 17 },
];

// map → naya array (Dart map jaisa)
const names = users.map((u) => u.name);
// ["Ali", "Sara", "Omar"]

// filter
const adults = users.filter((u) => u.age >= 18);

// find → pehla match (ya undefined)
const sara = users.find((u) => u.name === "Sara");

// findIndex
const i = users.findIndex((u) => u.id === 2);

// some / every
users.some((u) => u.age < 18);   // true
users.every((u) => u.age > 10);  // true

// reduce
const totalAge = users.reduce((sum, u) => sum + u.age, 0);
```

## 3.2 Objects = Dart Map / class instance

```js
const user = {
  name: "Ali",
  age: 20,
  city: "Lahore",
};

user.name;
user["age"];
user.email = "ali@mail.com";   // add
delete user.city;              // remove
```

### Destructuring (bahut useful)

```js
const { name, age } = user;
const [first, second] = ["a", "b", "c"];
```

### Spread

```js
const updated = { ...user, age: 21 };     // copy + change
const more = [...nums, 99];               // array copy + add
```

## Tasks — Day 3

Products array banao:

```js
const products = [
  { id: 1, title: "Phone", price: 500 },
  { id: 2, title: "Laptop", price: 1200 },
  { id: 3, title: "Mouse", price: 25 },
];
```

1. Sirf titles ka array (`map`)  
2. Price > 100 wale (`filter`)  
3. `id === 2` wala product (`find`)  
4. Total price (`reduce`)  
5. Spread se ek product ka price update karke naya object banao  

---

# DAY 4 — Conditions & Loops

## 4.1 if / else / ternary

```js
const age = 18;

if (age >= 18) {
  console.log("Adult");
} else {
  console.log("Minor");
}

const label = age >= 18 ? "Adult" : "Minor";
```

### Equality (important)

```js
2 == "2";   // true  (loose — avoid)
2 === "2";  // false (strict — hamesha ye use karo)
```

## 4.2 Loops

```js
for (let i = 0; i < 5; i++) {
  console.log(i);
}

const arr = ["a", "b", "c"];

for (const item of arr) {
  console.log(item);
}

arr.forEach((item, index) => {
  console.log(index, item);
});
```

Object keys:

```js
const user = { name: "Ali", age: 20 };

for (const key in user) {
  console.log(key, user[key]);
}

Object.keys(user);
Object.values(user);
Object.entries(user); // [["name","Ali"], ["age",20]]
```

## Tasks — Day 4

1. 1 se 20 tak print — even numbers only  
2. Array of scores: average nikaalo  
3. Grade function:  
   - 90+ A  
   - 80+ B  
   - 70+ C  
   - else F  

---

# DAY 5 — Async JS (Node.js ka dil)

Flutter mein `Future` / `async` / `await` aata hai.  
JS mein almost same — lekin pehle **Promise** samjho.

## 5.1 Promise = Future

```dart
// Dart
Future<String> fetchName() async {
  return 'Ali';
}
```

```js
// JavaScript
function fetchName() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("Ali");
      // reject(new Error("Failed"));
    }, 1000);
  });
}
```

## 5.2 then / catch (purana style)

```js
fetchName()
  .then((name) => console.log(name))
  .catch((err) => console.error(err));
```

## 5.3 async / await (yehi use karna — Node mein)

```js
async function main() {
  try {
    const name = await fetchName();
    console.log(name);
  } catch (error) {
    console.error(error.message);
  }
}

main();
```

### Real-world feel (fake API)

```js
function getUser(id) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (id === 1) {
        resolve({ id: 1, name: "Ali" });
      } else {
        reject(new Error("User not found"));
      }
    }, 500);
  });
}

async function run() {
  try {
    const user = await getUser(1);
    console.log(user);
  } catch (e) {
    console.error(e.message);
  }
}

run();
```

**Node + Postgres** mein yahi pattern:

```js
await sequelize.authenticate();
await User.findAll();
await User.create({ ... });
```

## Tasks — Day 5

1. `delay(ms)` function banao jo Promise return kare  
2. `async` function mein 1 second wait karke `"Done"` print karo  
3. `getUser(id)` banao — id 1 success, warna error  
4. `try/catch` se dono cases test karo  

---

# DAY 6 — Modules (files todna)

Flutter mein alag files / packages.  
JS mein bhi modules hote hain.

## 6.1 Two styles

### A) CommonJS (Node default — tumhare project mein yehi)

```js
// math.js
function add(a, b) {
  return a + b;
}
module.exports = { add };

// app.js
const { add } = require("./math");
console.log(add(2, 3));
```

### B) ES Modules (`import` / `export`)

```js
// math.js
export function add(a, b) {
  return a + b;
}

// app.js
import { add } from "./math.js";
```

Abhi backend seekhte waqt **CommonJS (`require` / `module.exports`)** pe focus rakho — tumhara `my-backend` isi pe hai.

## Tasks — Day 6

1. `utils.js` banao: `isEmail(str)`, `capitalize(str)`  
2. `app.js` mein require karke use karo  
3. `users.js` mein array export karo, `app.js` mein filter karke adults print karo  

---

# DAY 7 — Mini Projects (JS mastery)

In projects ko **browser console** ya **Node** se chalao.

### Project A — Todo (array only)

Features:
- add todo  
- list todos  
- delete by id  
- mark complete  

Hint structure:

```js
let todos = [];
let nextId = 1;

function addTodo(title) { ... }
function listTodos() { ... }
function deleteTodo(id) { ... }
function toggleTodo(id) { ... }
```

### Project B — User Manager (CRUD in memory)

```js
let users = [];

function createUser(name, email) { ... }
function getUsers() { ... }
function getUserById(id) { ... }
function updateUser(id, data) { ... }
function deleteUser(id) { ... }
```

Yeh bilkul wahi logic hai jo baad mein **controller** mein hogi — bas DB ki jagah array.

### Project C — Fake async API

`getUsers()` Promise return kare (setTimeout 500ms), phir `async/await` se print karo.

---

# Dart vs JavaScript Cheatsheet

| Concept | Dart | JavaScript |
|---------|------|------------|
| Final value | `final` / `const` | `const` |
| Mutable var | `var` / type | `let` |
| String interp | `'$name'` | `` `${name}` `` |
| List | `List` / `[]` | `Array` / `[]` |
| Map | `Map` / `{}` | `Object` / `{}` |
| Null | `null` | `null` / `undefined` |
| Async | `Future` | `Promise` |
| await | `await` | `await` |
| Arrow | `=>` | `=>` |
| print | `print()` | `console.log()` |
| Strict equal | `==` | `===` |
| Package import | `import '...'` | `require` / `import` |

---

# Common beginner mistakes

1. `==` use karna → hamesha `===`  
2. `const` array ko reassign karna (`arr = []`) — push/pop OK hai  
3. `async` function ke baghair `await`  
4. Promise ko `await` kiye baghair use karna  
5. `map` se original array change expect karna — `map` naya array deta hai  
6. `find` null nahi, **`undefined`** return karta hai  

---

# Practice Checklist (complete karo phir Node pe jao)

- [ ] `const` / `let` clear hai  
- [ ] Arrow functions likh sakta hoon  
- [ ] `map` / `filter` / `find` / `reduce` use kar sakta hoon  
- [ ] Object destructuring + spread aata hai  
- [ ] `async/await` + `try/catch` comfortable hai  
- [ ] `require` / `module.exports` se 2 files connect kar sakta hoon  
- [ ] In-memory CRUD (create/read/update/delete + getById) bana liya  

Jab ye checklist complete ho — **Node.js + Express + PostgreSQL** start karo.  
Us waqt tumhara pehla backend flow ye hoga:

```text
Route → Controller → Model → Database
```

Aur controller ke andar wahi `async/await` + array methods wala dimagh kaam karega.

---

# Daily study plan (suggested)

| Time | Kaam |
|------|------|
| 20 min | Section padho |
| 30 min | Examples type karo (copy-paste mat) |
| 30–40 min | Tasks / mini project |

---

# Next after this doc

1. Node.js basics (`npm`, `require`, simple server)  
2. Express routes  
3. PostgreSQL + Sequelize  
4. Real CRUD APIs (`GET`, `GET/:id`, `POST`, `PUT`, `DELETE`)

Jab JS checklist complete ho, bolo — **Node.js + PostgreSQL beginner doc** bhi isi style mein bana dunga.
