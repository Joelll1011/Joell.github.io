<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Habit Tracker Terapi Pribadi</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');
  :root {
    --primary-color: #6200ee;
    --primary-color-light: #bb86fc;
    --background-color: #121212;
    --card-bg: #1e1e1e;
    --text-color: #e0e0e0;
    --accent-color: #03dac6;
    --error-color: #cf6679;
    --border-radius: 12px;
    --spacing-small: 8px;
    --spacing-medium: 16px;
    --spacing-large: 32px;
    --shadow: 0 4px 8px rgba(0,0,0,0.6);
    --font-family: 'Inter', sans-serif;
  }
  /* Reset and base */
  * {
    box-sizing: border-box;
  }
  body {
    margin: 0;
    background: var(--background-color);
    color: var(--text-color);
    font-family: var(--font-family);
    line-height: 1.5;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    user-select: none;
  }
  a {
    color: var(--accent-color);
    text-decoration: none;
  }
  a:focus, button:focus, input:focus {
    outline: 3px solid var(--accent-color);
    outline-offset: 2px;
  }
  h1, h2, h3 {
    font-weight: 600;
    margin-bottom: var(--spacing-small);
  }
  /* Layout */
  .container {
    max-width: 1024px;
    margin: 0 auto;
    padding: var(--spacing-large);
  }
  header {
    text-align: center;
    padding-bottom: var(--spacing-medium);
    border-bottom: 1px solid #333;
  }
  header h1 {
    font-size: 2rem;
    background: linear-gradient(135deg, var(--primary-color-light), var(--accent-color));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    cursor: default;
  }
  main {
    margin-top: var(--spacing-large);
  }
  /* Habit input */
  form#habit-form {
    display: flex;
    flex-wrap: wrap;
    gap: var(--spacing-medium);
    margin-bottom: var(--spacing-large);
  }
  form#habit-form input[type="text"] {
    flex: 1 1 200px;
    padding: var(--spacing-medium);
    border-radius: var(--border-radius);
    border: none;
    font-size: 1rem;
    background: var(--card-bg);
    color: var(--text-color);
    box-shadow: var(--shadow);
    transition: box-shadow 0.3s ease;
  }
  form#habit-form input[type="text"]:focus {
    box-shadow: 0 0 8px var(--accent-color);
  }
  form#habit-form button {
    background: var(--primary-color);
    border: none;
    color: white;
    font-weight: 600;
    font-size: 1rem;
    border-radius: var(--border-radius);
    padding: var(--spacing-medium) var(--spacing-large);
    cursor: pointer;
    transition: background-color 0.3s ease;
    box-shadow: var(--shadow);
  }
  form#habit-form button:hover,
  form#habit-form button:focus {
    background: var(--primary-color-light);
    outline: none;
  }
  /* Habit list */
  ul#habit-list {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  li.habit-item {
    background: var(--card-bg);
    border-radius: var(--border-radius);
    padding: var(--spacing-medium);
    margin-bottom: var(--spacing-medium);
    box-shadow: var(--shadow);
    display: grid;
    grid-template-columns: 1fr auto;
    align-items: center;
    gap: var(--spacing-medium);
  }
  li.habit-item .habit-details {
    overflow-wrap: break-word;
  }
  li.habit-item .habit-name {
    font-weight: 600;
    font-size: 1.15rem;
    margin-bottom: 4px;
    user-select: text;
  }
  li.habit-item .habit-controls {
    display: flex;
    align-items: center;
    gap: var(--spacing-small);
  }
  button.check-btn {
    width: 52px;
    height: 52px;
    border-radius: 50%;
    border: 2px solid var(--accent-color);
    background: transparent;
    color: var(--accent-color);
    cursor: pointer;
    font-size: 1.8rem;
    line-height: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background-color 0.3s ease, color 0.3s ease;
  }
  button.check-btn.checked {
    background: var(--accent-color);
    color: var(--background-color);
  }
  button.delete-btn {
    background: transparent;
    border: none;
    color: var(--error-color);
    font-size: 1.5rem;
    cursor: pointer;
    transition: color 0.2s ease;
  }
  button.delete-btn:hover,
  button.delete-btn:focus {
    color: #ff5252;
    outline: none;
  }
  /* Habit history modal */
  .modal {
    position: fixed;
    inset: 0;
    background: rgba(18, 18, 18, 0.9);
    display: flex;
    justify-content: center;
    align-items: center;
    padding: var(--spacing-large);
    z-index: 9999;
    visibility: hidden;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.3s ease;
  }
  .modal.active {
    visibility: visible;
    opacity: 1;
    pointer-events: auto;
  }
  .modal-content {
    background: var(--card-bg);
    border-radius: var(--border-radius);
    max-width: 600px;
    max-height: 80vh;
    overflow-y: auto;
    padding: var(--spacing-large);
    box-shadow: var(--shadow);
    display: flex;
    flex-direction: column;
  }
  .modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: var(--spacing-medium);
  }
  .modal-header h2 {
    margin: 0;
    font-weight: 700;
  }
  button.close-btn {
    background: transparent;
    border: none;
    color: var(--accent-color);
    font-size: 1.8rem;
    cursor: pointer;
    transition: color 0.3s ease;
  }
  button.close-btn:hover,
  button.close-btn:focus {
    color: var(--primary-color-light);
    outline: none;
  }
  .history-list {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  .history-list li {
    display: flex;
    justify-content: space-between;
    padding: var(--spacing-small) 0;
    border-bottom: 1px solid #333;
    font-size: 0.9rem;
    color: var(--accent-color);
  }
  .history-list li.done {
    color: var(--accent-color);
  }
  .history-list li.missed {
    color: var(--error-color);
    text-decoration: line-through;
  }
  /* Responsive */
  @media (max-width: 767px) {
    .container {
      padding: var(--spacing-medium);
    }
    form#habit-form {
      flex-direction: column;
    }
    form#habit-form input[type="text"] {
      flex: none;
      width: 100%;
    }
    form#habit-form button {
      width: 100%;
    }
    li.habit-item {
      grid-template-columns: 1fr 48px 48px;
      gap: var(--spacing-small);
    }
  }
</style>
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet" />
</head>
<body>
  <header>
    <h1>Habit Tracker Terapi Pribadi</h1>
  </header>
  <main class="container" role="main">
    <form id="habit-form" aria-label="Form tambah kegiatan baru">
      <input type="text" id="habit-input" placeholder="Tambah kegiatan baru..." aria-label="Masukkan nama kegiatan" required autocomplete="off" />
      <button type="submit" aria-label="Tambahkan kegiatan baru">Tambah</button>
    </form>
    <ul id="habit-list" aria-live="polite" aria-relevant="additions"></ul>
  </main>

  <div class="modal" id="history-modal" role="dialog" aria-modal="true" aria-labelledby="history-title">
    <div class="modal-content">
      <div class="modal-header">
        <h2 id="history-title">Riwayat Kegiatan</h2>
        <button class="close-btn" aria-label="Tutup riwayat kegiatan" id="close-history-btn">
          <span class="material-icons">close</span>
        </button>
      </div>
      <ul class="history-list" id="history-list"></ul>
    </div>
  </div>

  <script>
    (() => {
      const habitForm = document.getElementById('habit-form');
      const habitInput = document.getElementById('habit-input');
      const habitList = document.getElementById('habit-list');
      const historyModal = document.getElementById('history-modal');
      const closeHistoryBtn = document.getElementById('close-history-btn');
      const historyList = document.getElementById('history-list');

      const STORAGE_KEY = 'therapyHabitTrackerData';

      let habits = {};

      // Save habits object to localStorage
      function saveHabits() {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(habits));
      }

      // Load habits from localStorage
      function loadHabits() {
        const data = localStorage.getItem(STORAGE_KEY);
        if (data) {
          habits = JSON.parse(data);
        }
      }

      // Format date as yyyy-mm-dd
      function formatDate(date) {
        return date.toISOString().split('T')[0];
      }

      // Render all habits on screen
      function renderHabits() {
        habitList.innerHTML = '';
        const today = formatDate(new Date());

        Object.entries(habits).forEach(([id, habit]) => {
          // Create main list item
          const li = document.createElement('li');
          li.className = 'habit-item';
          li.setAttribute('data-id', id);

          // Habit details container
          const details = document.createElement('div');
          details.className = 'habit-details';

          const habitName = document.createElement('div');
          habitName.className = 'habit-name';
          habitName.textContent = habit.name;
          details.appendChild(habitName);

          li.appendChild(details);

          // Controls container
          const controls = document.createElement('div');
          controls.className = 'habit-controls';

          // Done toggle button for today
          const checkBtn = document.createElement('button');
          checkBtn.className = 'check-btn';
          checkBtn.setAttribute('aria-label', `Tandai '${habit.name}' selesai hari ini`);
          checkBtn.title = `Tandai selesai hari ini ("${habit.name}")`;
          // Check if today's date is marked done
          if (habit.history && habit.history[today]) {
            checkBtn.classList.add('checked');
            checkBtn.setAttribute('aria-pressed', 'true');
          } else {
            checkBtn.setAttribute('aria-pressed', 'false');
          }
          checkBtn.innerHTML = '<span class="material-icons">check</span>';

          checkBtn.addEventListener('click', () => {
            toggleDone(id);
          });
          controls.appendChild(checkBtn);

          // History button
          const historyBtn = document.createElement('button');
          historyBtn.className = 'check-btn';
          historyBtn.setAttribute('aria-label', `Lihat riwayat kegiatan '${habit.name}'`);
          historyBtn.title = `Lihat riwayat kegiatan ("${habit.name}")`;
          historyBtn.innerHTML = '<span class="material-icons">history</span>';
          historyBtn.style.borderColor = 'var(--primary-color-light)';
          historyBtn.style.color = 'var(--primary-color-light)';
          historyBtn.addEventListener('click', () => {
            openHistory(id);
          });
          controls.appendChild(historyBtn);

          // Delete button
          const deleteBtn = document.createElement('button');
          deleteBtn.className = 'delete-btn';
          deleteBtn.setAttribute('aria-label', `Hapus kegiatan '${habit.name}'`);
          deleteBtn.title = `Hapus kegiatan ("${habit.name}")`;
          deleteBtn.innerHTML = '<span class="material-icons">delete</span>';
          deleteBtn.addEventListener('click', () => {
            if (confirm(`Yakin ingin menghapus kegiatan "${habit.name}"?`)) {
              deleteHabit(id);
            }
          });
          controls.appendChild(deleteBtn);

          li.appendChild(controls);

          habitList.appendChild(li);
        });

        if (Object.keys(habits).length === 0) {
          habitList.innerHTML = '<p style="text-align:center; color:#666;">Belum ada kegiatan, tambahkan di atas.</p>';
        }
      }

      // Add new habit
      habitForm.addEventListener('submit', (e) => {
        e.preventDefault();
        const name = habitInput.value.trim();
        if (!name) return;
        const id = 'habit-' + Date.now();
        habits[id] = { name, history: {} };
        saveHabits();
        renderHabits();
        habitInput.value = '';
        habitInput.focus();
      });

      // Toggle today's done state for given habit id
      function toggleDone(id) {
        const today = formatDate(new Date());
        if (!habits[id]) return;
        if (!habits[id].history) {
          habits[id].history = {};
        }
        if (habits[id].history[today]) {
          delete habits[id].history[today];
        } else {
          habits[id].history[today] = true;
        }
        saveHabits();
        renderHabits();
      }

      // Delete habit
      function deleteHabit(id) {
        delete habits[id];
        saveHabits();
        renderHabits();
      }

      // Open history modal for given habit id
      function openHistory(id) {
        const habit = habits[id];
        if (!habit) return;
        historyList.innerHTML = '';
        document.getElementById('history-title').textContent = `Riwayat Kegiatan: ${habit.name}`;
        // List last 30 days
        const daysToShow = 30;
        const today = new Date();
        for (let i = daysToShow - 1; i >= 0; i--) {
          const day = new Date(today);
          day.setDate(today.getDate() - i);
          const dayStr = formatDate(day);
          const li = document.createElement('li');
          li.textContent = dayStr;
          if (habit.history && habit.history[dayStr]) {
            li.classList.add('done');
            li.textContent += ' - Selesai';
          } else {
            li.classList.add('missed');
            li.textContent += ' - Belum';
          }
          historyList.appendChild(li);
        }
        historyModal.classList.add('active');
        // Trap focus inside modal
        trapFocus(historyModal);
      }

      // Close history modal
      closeHistoryBtn.addEventListener('click', () => {
        historyModal.classList.remove('active');
      });

      // Close modal on ESC
      window.addEventListener('keydown', (e) => {
        if (e.key === 'Escape' && historyModal.classList.contains('active')) {
          historyModal.classList.remove('active');
        }
      });

      // Focus trap utility for modal
      function trapFocus(element) {
        const focusableElements = element.querySelectorAll('a[href], button:not([disabled]), textarea, input, select');
        if (focusableElements.length === 0) return;
        const firstEl = focusableElements[0];
        const lastEl = focusableElements[focusableElements.length -1];
        firstEl.focus();

        element.addEventListener('keydown', function(e) {
          if (e.key === 'Tab' || e.keyCode === 9) {
            if (e.shiftKey) { // Shift + Tab
              if (document.activeElement === firstEl) {
                e.preventDefault();
                lastEl.focus();
              }
            } else { // Tab
              if (document.activeElement === lastEl) {
                e.preventDefault();
                firstEl.focus();
              }
            }
          }
        }, { once: true });
      }

      // Initialize
      loadHabits();
      renderHabits();
    })();
  </script>
</body>
</html>

