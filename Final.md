## Key features 🚀

### 🧩 Interactive Data Entry via `prompt()`
Users input their **name** and **age** through native browser dialogs, keeping the interaction lightweight and dependency-free. Variables are declared with `const` and `let` — no `var` in sight.

```js
const userName = prompt("Welcome! What is your name?");
const userAge  = Number(prompt(`Hi!! ${userName} How old are you?`));
` `` `

### ✅ Age Validation & Error Handling
The system checks that the age entered is a **valid positive integer** using `isNaN()`, `Number.isInteger()`, and a greater-than-zero guard. Invalid input triggers `console.error()` for developer feedback and a visible **toast notification** on screen — no silent failures.

```js
if (isNaN(userAge) || userAge <= 0 || !Number.isInteger(userAge)) {
  console.error("Error: Please enter a valid age in numbers.");
  showToast();
  return;
}
` `` `

### 💬 Dynamic Conditional Messaging
After validation, an `if / else` block delivers a **personalized message** based on age:
- **Under 18** → encouraging message for young coders via `alert()` + `console.log()`
- **18 and over** → motivational message for adult users

```js
if (userAge < 18) {
  const mensajeMenor = `Hi ${userName}, you are a minor. Keep learning and enjoying coding!`;
  alert(mensajeMenor);
  console.log(mensajeMenor);
} else {
  const mensajeMayor = `Hi ${userName}, get ready for great opportunities in the world of programming!`;
  alert(mensajeMayor);
  console.log(mensajeMayor);
}
` `` `

### 📋 Real-Time Table Management (CRUD)
Every registered user is stored in a **local `users` array** and rendered as a table row with auto-incremental ID, age category badge, status badge, and a delete button.

```js
users.push({ id: idCounter, name: userName, age: userAge, state: true });
renderRow(idCounter, userName, userAge);
idCounter++;
userCount.textContent = users.length;
` `` `

### 🗑️ Animated Row Deletion
Clicking **✕ Delete** triggers a CSS exit animation (`removing` class), then removes the row from the DOM and splices it from the `users` array after `300ms`.

```js
row.classList.add('removing');
setTimeout(() => {
  row.remove();
  const index = users.findIndex(user => user.id === id);
  if (index !== -1) users.splice(index, 1);
  userCount.textContent = users.length;
}, 300);
` `` `

### 🔢 Live User Counter
The header count updates automatically on every **add** and **delete** action, always reflecting the current number of registered users.

```js
userCount.textContent = users.length;
` `` `

### 🌑 Dark Mode UI with CSS Custom Properties
All colors are defined once in `:root` as CSS custom properties and reused throughout — making the neon-on-dark theme consistent and easy to maintain.

```css
:root {
  --color-bg:      #0d0d0d;
  --color-accent:  #00ffe0;
  --color-surface: #1a1a1a;
}
` `` `
