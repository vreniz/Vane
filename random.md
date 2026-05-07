que necesito de aca para hacer el showtoast en cualquier proyecto: // ============================================================
// GLOBAL VARIABLES
// ============================================================
const users = [];      // Array to store user objects
let idCounter = 0;     // Counter for auto-incremental IDs

// References to DOM elements
const tbody      = document.getElementById('table_body');
const emptyState = document.getElementById('empty-state');
const userCount  = document.getElementById('user-count');
const toast      = document.getElementById('toast');


// TRANSITION: WELCOME → MAIN PAGE

const startApp = () => {
  document.getElementById('welcome-page').style.display = 'none';
  document.getElementById('main-page').style.display    = 'block';
};



// TRANSITION: MAIN PAGE → WELCOME (and reset everything)

const resetApp = () => {
  // Clear the array and reset the ID counter
  users.length = 0;
  idCounter    = 0;

  // Clear the table rows and show the empty state again
  tbody.innerHTML          = '';
  emptyState.style.display = 'block';
  userCount.textContent    = '0';

  // Go back to the welcome page
  document.getElementById('main-page').style.display    = 'none';
  document.getElementById('welcome-page').style.display = 'flex';

  console.log('System reset. Users array:', users);
};



// MAIN FUNCTION: ADD USER

const addUser = () => {
  

  // Capture name with prompt
  const userName = prompt("Welcome! What is your name?");

  // If the user cancels the prompt, exit without doing anything
  if (userName === null) {
    return;
  }

  const userAge = Number(prompt(`Hi!! ${userName} How old are you?`));

  // Validate: age must be a number and greater than 0
  if (isNaN(userAge) || userAge <= 0 || !Number.isInteger(userAge)) {
    console.error("Error: Please enter a valid age in numbers.");
    showToast();
    return;
  }

  // Show dynamic message based on age
  if (userAge < 18) {
    const mensajeMenor = `Hi ${userName}, you are a minor. Keep learning and enjoying coding!`;
    alert(mensajeMenor);
    console.log(mensajeMenor);
  } else {
    const mensajeMayor = `Hi ${userName}, get ready for great opportunities in the world of programming!`;
    alert(mensajeMayor);
    console.log(mensajeMayor);
  }

  // Save user to the array
  users.push({ id: idCounter, name: userName, age: userAge, state: true });
  console.log(users);

  // Render the new row in the table
  renderRow(idCounter, userName, userAge);
  idCounter++;

  // Update visible counter and hide empty state
  userCount.textContent    = users.length;
  emptyState.style.display = 'none';

};


// RENDER A ROW IN THE TABLE
// Includes: ID, Name, Age, Category badge, Status badge, Delete button
const renderRow = (id, name, age) => {
  const tr = document.createElement('tr');

  // Determine category based on age
  const isMinor    = age < 18;
  const category   = isMinor ? 'Minor'  : 'Adult';
  const badgeClass = isMinor ? 'badge-menor' : 'badge-mayor';

  tr.innerHTML = `
    <td>#${String(id).padStart(3, '0')}</td>
    <td>${name}</td>
    <td>${age}</td>
    <td><span class="${badgeClass}">${category}</span></td>
    <td><span class="badge">Active</span></td>
    <td>
      <button class="btn-delete" onclick="deleteRow(this, ${id})">
        ✕ Delete
      </button>
    </td>
  `;

  tbody.appendChild(tr);
};


// DELETE A ROW FROM THE TABLE
// Removes the row visually and from the users array
const deleteRow = (btn, id) => {
  // Find the <tr> that contains the clicked button
  const row = btn.closest('tr');

  // Add the CSS exit animation class
  row.classList.add('removing');

  // Wait for the animation to finish, then remove the element from the DOM
  setTimeout(() => {
    row.remove();

    // Remove the user from the array by matching the ID
    const index = users.findIndex(user => user.id === id);
    if (index !== -1) users.splice(index, 1);

    // Update the visible counter
    userCount.textContent = users.length;

    // If no users remain, show the empty state again
    if (users.length === 0) {
      emptyState.style.display = 'block';
    }

    console.log(`User #${String(id).padStart(3,'0')} deleted. Remaining:`, users);
  }, 300); // 300ms matches the CSS transition duration
};



// SHOW ERROR TOAST ON SCREEN

const showToast = () => {
  toast.classList.add('show');
  setTimeout(() => toast.classList.remove('show'), 3000);
};
  <!-- Toast for error -->
  <div id="toast">⚠️ Error: enter a valid age number.</div>

  <script src="interactive_sis.js"></script>
</body>
</html>
/* ============================================================
   TOAST DE ERROR
============================================================ */
#toast {
  position: fixed;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%) translateY(100px);
  background: var(--error);
  color: #fff;
  padding: 0.8rem 1.6rem;
  border-radius: 3px;
  font-size: 0.82rem;
  letter-spacing: 0.05em;
  transition: transform 0.35s cubic-bezier(.175,.885,.32,1.275);
  z-index: 999;
  white-space: nowrap;
}

#toast.show {
  transform: translateX(-50%) translateY(0);
}Para llevarte el showToast a cualquier otro proyecto necesitas "la triada": HTML (la estructura), CSS (la animación y estilo) y JS (la lógica).Aquí tienes lo esencial extraído y limpio:1. El HTML (Estructura)Pon esto al final de tu <body>:html<div id="toast">⚠️ Error: enter a valid age number.</div>
Usa el código con precaución.2. El CSS (Diseño y Animación)Asegúrate de definir el color --error o cambiarlo por uno fijo (como red o #ff4b2b):css#toast {
  position: fixed;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%) translateY(100px); /* Escondido abajo */
  background: #ff4b2b; /* Color de fondo */
  color: #fff;
  padding: 0.8rem 1.6rem;
  border-radius: 8px;
  transition: transform 0.35s cubic-bezier(.175,.885,.32,1.275);
  z-index: 999;
  visibility: hidden; /* Evita clics accidentales cuando está oculto */
}

#toast.show {
  transform: translateX(-50%) translateY(0); /* Sube a la pantalla */
  visibility: visible;
}
Usa el código con precaución.3. El JavaScript (Lógica)Necesitas la referencia al elemento y la función que añade/quita la clase:javascriptconst toast = document.getElementById('toast');

const showToast = () => {
  toast.classList.add('show');
  
  // Se quita solo después de 3 segundos
  setTimeout(() => {
    toast.classList.remove('show');
  }, 3000);
};
Usa el código con precaución.Pro-Tip: Hazlo dinámicoSi quieres usarlo para cualquier mensaje (no solo errores de edad), modifica la función así:javascriptconst showToast = (mensaje) => {
  toast.textContent = mensaje; // Cambia el texto al vuelo
  toast.classList.add('show');
  setTimeout(() => toast.classList.remove('show'), 3000);
};

// Uso:
showToast("¡Usuario guardado con éxito!");
showToast("⚠️ Error: Nombre muy corto");
Usa el código con precaución.
