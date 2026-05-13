# Ex03 To-Do List using JavaScript
## Date: 13/05/2026

## AIM
To create a To-do Application with all features using JavaScript.

## ALGORITHM
### STEP 1
Build the HTML structure (index.html).

### STEP 2
Style the App (style.css).

### STEP 3
Plan the features the To-Do App should have.

### STEP 4
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

### STEP 6
Test the App.

### STEP 7
Open the HTML file in a browser to check layout and functionality.

### STEP 8
Fix styling issues and refine content placement.

### STEP 9
Deploy the website.

### STEP 10
Upload to GitHub Pages for free hosting.

## PROGRAM
#index.html
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Ex03 — To-Do List</title>
  <link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Mono:wght@300;400;500&display=swap" rel="stylesheet"/>
  <link rel="stylesheet" href="style.css"/>
</head>
<body>

  <div class="bg-grid"></div>

  <div class="container">
    <header>
      <div class="header-badge">EX — 03</div>
      <h1>Task<span>Board</span></h1>
      <p class="subtitle">JavaScript To-Do Application</p>
    </header>

    <!-- Stats Bar -->
    <div class="stats-bar">
      <div class="stat">
        <span class="stat-num" id="totalCount">0</span>
        <span class="stat-label">Total</span>
      </div>
      <div class="stat">
        <span class="stat-num" id="activeCount">0</span>
        <span class="stat-label">Active</span>
      </div>
      <div class="stat">
        <span class="stat-num" id="doneCount">0</span>
        <span class="stat-label">Done</span>
      </div>
      <div class="progress-wrap">
        <div class="progress-bar"><div class="progress-fill" id="progressFill"></div></div>
        <span class="progress-label" id="progressLabel">0%</span>
      </div>
    </div>

    <!-- Input Area -->
    <div class="input-area">
      <div class="input-row">
        <input type="text" id="taskInput" placeholder="Add a new task…" maxlength="120" autocomplete="off"/>
        <select id="prioritySelect">
          <option value="low">🟢 Low</option>
          <option value="medium" selected>🟡 Medium</option>
          <option value="high">🔴 High</option>
        </select>
        <input type="date" id="dueDateInput" title="Due date (optional)"/>
        <button id="addBtn" onclick="addTask()">+ Add</button>
      </div>
    </div>

    <!-- Filter & Sort Bar -->
    <div class="filter-bar">
      <div class="filter-tabs">
        <button class="filter-tab active" onclick="setFilter('all', this)">All</button>
        <button class="filter-tab" onclick="setFilter('active', this)">Active</button>
        <button class="filter-tab" onclick="setFilter('completed', this)">Done</button>
        <button class="filter-tab" onclick="setFilter('high', this)">🔴 High</button>
      </div>
      <div class="sort-group">
        <label>Sort:</label>
        <select id="sortSelect" onchange="renderTasks()">
          <option value="newest">Newest</option>
          <option value="oldest">Oldest</option>
          <option value="priority">Priority</option>
          <option value="alpha">A–Z</option>
          <option value="duedate">Due Date</option>
        </select>
      </div>
    </div>

    <!-- Search -->
    <div class="search-row">
      <input type="text" id="searchInput" placeholder="🔍  Search tasks…" oninput="renderTasks()"/>
      <button class="clear-all-btn" onclick="clearCompleted()">Clear Done</button>
    </div>

    <!-- Task List -->
    <ul class="task-list" id="taskList"></ul>

    <!-- Empty State -->
    <div class="empty-state" id="emptyState">
      <div class="empty-icon">📋</div>
      <p>No tasks yet.<br/>Add your first task above!</p>
    </div>

    <footer>
      <span>JavaScript To-Do App &nbsp;·&nbsp; Ex03</span>
      <button onclick="exportTasks()" class="export-btn">⬇ Export JSON</button>
    </footer>
  </div>

  <script src="script.js"></script>
</body>
</html>
```

#style.css
```
/* ── Reset & Base ───────────────────────────────── */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --bg:       #0d0f14;
  --surface:  #151820;
  --card:     #1c2030;
  --border:   #2a2f42;
  --accent:   #e8ff47;
  --accent2:  #47c8ff;
  --text:     #f0f2f8;
  --muted:    #6b7280;
  --green:    #4ade80;
  --yellow:   #fbbf24;
  --red:      #f87171;
  --radius:   12px;
  --mono:     'DM Mono', monospace;
  --sans:     'Syne', sans-serif;
}

body {
  background: var(--bg);
  color: var(--text);
  font-family: var(--sans);
  min-height: 100vh;
  padding: 40px 20px 80px;
  overflow-x: hidden;
}

/* ── Bg Grid ────────────────────────────────────── */
.bg-grid {
  position: fixed; inset: 0; z-index: 0; pointer-events: none;
  background-image:
    linear-gradient(rgba(232,255,71,.04) 1px, transparent 1px),
    linear-gradient(90deg, rgba(232,255,71,.04) 1px, transparent 1px);
  background-size: 40px 40px;
}

/* ── Container ──────────────────────────────────── */
.container {
  max-width: 760px;
  margin: 0 auto;
  position: relative; z-index: 1;
}

/* ── Header ─────────────────────────────────────── */
header { text-align: center; margin-bottom: 36px; }

.header-badge {
  display: inline-block;
  font-family: var(--mono);
  font-size: 11px;
  letter-spacing: 3px;
  color: var(--accent);
  border: 1px solid var(--accent);
  padding: 4px 12px;
  border-radius: 40px;
  margin-bottom: 14px;
  text-transform: uppercase;
}

h1 {
  font-size: clamp(2.4rem, 7vw, 4rem);
  font-weight: 800;
  letter-spacing: -2px;
  line-height: 1;
  color: var(--text);
}
h1 span { color: var(--accent); }

.subtitle {
  font-family: var(--mono);
  font-size: 13px;
  color: var(--muted);
  margin-top: 8px;
}

/* ── Stats Bar ──────────────────────────────────── */
.stats-bar {
  display: flex;
  align-items: center;
  gap: 28px;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 16px 24px;
  margin-bottom: 20px;
}

.stat { text-align: center; }
.stat-num {
  display: block;
  font-size: 1.5rem;
  font-weight: 800;
  color: var(--accent);
  line-height: 1;
}
.stat-label {
  font-family: var(--mono);
  font-size: 10px;
  color: var(--muted);
  text-transform: uppercase;
  letter-spacing: 1px;
}

.progress-wrap { margin-left: auto; display: flex; align-items: center; gap: 10px; min-width: 160px; }
.progress-bar {
  flex: 1; height: 6px; background: var(--border); border-radius: 99px; overflow: hidden;
}
.progress-fill {
  height: 100%; width: 0%; background: var(--accent);
  border-radius: 99px; transition: width .4s ease;
}
.progress-label {
  font-family: var(--mono); font-size: 12px; color: var(--accent); min-width: 32px;
}

/* ── Input Area ─────────────────────────────────── */
.input-area {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 16px;
  margin-bottom: 16px;
}

.input-row {
  display: flex; gap: 10px; flex-wrap: wrap;
}

.input-row input[type="text"] {
  flex: 1 1 200px;
  background: var(--card); border: 1px solid var(--border);
  color: var(--text); font-family: var(--sans); font-size: 15px;
  padding: 12px 16px; border-radius: 8px; outline: none;
  transition: border-color .2s;
}
.input-row input[type="text"]:focus { border-color: var(--accent); }

.input-row select,
.input-row input[type="date"] {
  background: var(--card); border: 1px solid var(--border);
  color: var(--text); font-family: var(--mono); font-size: 12px;
  padding: 10px 12px; border-radius: 8px; outline: none; cursor: pointer;
}
.input-row input[type="date"] { color-scheme: dark; }

#addBtn {
  background: var(--accent); color: #0d0f14;
  font-family: var(--sans); font-weight: 700; font-size: 14px;
  border: none; padding: 12px 22px; border-radius: 8px; cursor: pointer;
  white-space: nowrap; transition: opacity .2s, transform .1s;
}
#addBtn:hover { opacity: .85; }
#addBtn:active { transform: scale(.97); }

/* ── Filter Bar ─────────────────────────────────── */
.filter-bar {
  display: flex; align-items: center; gap: 12px; flex-wrap: wrap;
  margin-bottom: 10px;
}

.filter-tabs { display: flex; gap: 6px; }
.filter-tab {
  background: var(--surface); border: 1px solid var(--border);
  color: var(--muted); font-family: var(--mono); font-size: 12px;
  padding: 7px 14px; border-radius: 40px; cursor: pointer;
  transition: all .2s;
}
.filter-tab.active,
.filter-tab:hover { background: var(--accent); color: #0d0f14; border-color: var(--accent); }

.sort-group {
  margin-left: auto; display: flex; align-items: center; gap: 8px;
  font-family: var(--mono); font-size: 12px; color: var(--muted);
}
.sort-group select {
  background: var(--card); border: 1px solid var(--border);
  color: var(--text); font-family: var(--mono); font-size: 12px;
  padding: 7px 10px; border-radius: 8px; outline: none;
}

/* ── Search Row ─────────────────────────────────── */
.search-row {
  display: flex; gap: 10px; margin-bottom: 18px;
}
#searchInput {
  flex: 1; background: var(--surface); border: 1px solid var(--border);
  color: var(--text); font-family: var(--sans); font-size: 14px;
  padding: 10px 16px; border-radius: 8px; outline: none; transition: border-color .2s;
}
#searchInput:focus { border-color: var(--accent2); }

.clear-all-btn {
  background: transparent; border: 1px solid var(--border);
  color: var(--muted); font-family: var(--mono); font-size: 12px;
  padding: 10px 16px; border-radius: 8px; cursor: pointer; white-space: nowrap;
  transition: all .2s;
}
.clear-all-btn:hover { border-color: var(--red); color: var(--red); }

/* ── Task List ──────────────────────────────────── */
.task-list { list-style: none; display: flex; flex-direction: column; gap: 10px; }

.task-item {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 14px 16px;
  display: flex; align-items: flex-start; gap: 12px;
  transition: border-color .2s, opacity .3s;
  animation: slideIn .25s ease;
  position: relative; overflow: hidden;
}
.task-item::before {
  content: '';
  position: absolute; left: 0; top: 0; bottom: 0; width: 3px;
}
.task-item.priority-low::before    { background: var(--green); }
.task-item.priority-medium::before { background: var(--yellow); }
.task-item.priority-high::before   { background: var(--red); }

.task-item.completed { opacity: .5; }
.task-item:hover { border-color: rgba(232,255,71,.3); }

@keyframes slideIn {
  from { opacity: 0; transform: translateY(-8px); }
  to   { opacity: 1; transform: translateY(0); }
}

.task-check {
  appearance: none; -webkit-appearance: none;
  width: 20px; height: 20px; border-radius: 6px;
  border: 2px solid var(--border); background: transparent;
  cursor: pointer; flex-shrink: 0; margin-top: 1px;
  transition: all .2s; position: relative;
}
.task-check:checked {
  background: var(--accent); border-color: var(--accent);
}
.task-check:checked::after {
  content: '✓';
  position: absolute; top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  color: #0d0f14; font-size: 12px; font-weight: 800;
}

.task-body { flex: 1; min-width: 0; }

.task-text {
  font-size: 15px; font-weight: 600; word-break: break-word;
  transition: color .2s;
}
.completed .task-text { text-decoration: line-through; color: var(--muted); }

.task-meta {
  display: flex; align-items: center; gap: 8px; margin-top: 5px; flex-wrap: wrap;
}
.tag {
  font-family: var(--mono); font-size: 10px; letter-spacing: .5px;
  padding: 2px 8px; border-radius: 40px; text-transform: uppercase;
}
.tag-low    { background: rgba(74,222,128,.12); color: var(--green); }
.tag-medium { background: rgba(251,191,36,.12);  color: var(--yellow); }
.tag-high   { background: rgba(248,113,113,.12); color: var(--red); }

.task-date {
  font-family: var(--mono); font-size: 11px; color: var(--muted);
}
.task-due {
  font-family: var(--mono); font-size: 11px;
}
.task-due.overdue { color: var(--red); }
.task-due.today   { color: var(--yellow); }
.task-due.upcoming { color: var(--accent2); }

.task-actions { display: flex; gap: 6px; flex-shrink: 0; }
.task-btn {
  background: transparent; border: 1px solid var(--border);
  color: var(--muted); font-size: 13px;
  width: 30px; height: 30px; border-radius: 7px;
  cursor: pointer; display: flex; align-items: center; justify-content: center;
  transition: all .2s;
}
.task-btn.edit-btn:hover  { border-color: var(--accent2); color: var(--accent2); }
.task-btn.del-btn:hover   { border-color: var(--red); color: var(--red); }

/* Edit mode */
.task-edit-input {
  width: 100%; background: var(--surface); border: 1px solid var(--accent);
  color: var(--text); font-family: var(--sans); font-size: 15px; font-weight: 600;
  padding: 6px 10px; border-radius: 6px; outline: none; margin-top: 2px;
}

/* ── Empty State ────────────────────────────────── */
.empty-state {
  text-align: center; padding: 60px 20px; display: none;
}
.empty-icon { font-size: 3rem; margin-bottom: 16px; }
.empty-state p { font-family: var(--mono); color: var(--muted); font-size: 14px; line-height: 1.7; }

/* ── Footer ─────────────────────────────────────── */
footer {
  margin-top: 36px; display: flex; align-items: center; justify-content: space-between;
  font-family: var(--mono); font-size: 12px; color: var(--muted);
}
.export-btn {
  background: transparent; border: 1px solid var(--border);
  color: var(--muted); font-family: var(--mono); font-size: 12px;
  padding: 7px 14px; border-radius: 8px; cursor: pointer; transition: all .2s;
}
.export-btn:hover { border-color: var(--accent2); color: var(--accent2); }

/* ── Responsive ─────────────────────────────────── */
@media (max-width: 520px) {
  .stats-bar { flex-wrap: wrap; gap: 14px; }
  .progress-wrap { min-width: 100%; margin-left: 0; }
  .filter-bar { flex-direction: column; align-items: flex-start; }
  .sort-group { margin-left: 0; }
}
```
#script.js
```
// ── State ────────────────────────────────────────────
let tasks = JSON.parse(localStorage.getItem('ex03_tasks') || '[]');
let currentFilter = 'all';

// ── Helpers ──────────────────────────────────────────
function saveTasks() {
  localStorage.setItem('ex03_tasks', JSON.stringify(tasks));
}

function generateId() {
  return Date.now().toString(36) + Math.random().toString(36).slice(2, 6);
}

function formatDate(iso) {
  if (!iso) return '';
  const d = new Date(iso + 'T00:00:00');
  return d.toLocaleDateString('en-IN', { day: '2-digit', month: 'short', year: 'numeric' });
}

function getDueStatus(dueDate) {
  if (!dueDate) return null;
  const today = new Date(); today.setHours(0,0,0,0);
  const due   = new Date(dueDate + 'T00:00:00');
  if (due < today) return 'overdue';
  if (due.getTime() === today.getTime()) return 'today';
  return 'upcoming';
}

function escapeHTML(str) {
  return str.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
}

// ── Add Task ─────────────────────────────────────────
function addTask() {
  const input    = document.getElementById('taskInput');
  const priority = document.getElementById('prioritySelect').value;
  const dueDate  = document.getElementById('dueDateInput').value;
  const text     = input.value.trim();

  if (!text) {
    input.focus();
    input.style.borderColor = '#f87171';
    setTimeout(() => input.style.borderColor = '', 1000);
    return;
  }

  tasks.unshift({
    id:        generateId(),
    text,
    priority,
    dueDate,
    completed: false,
    createdAt: new Date().toISOString()
  });

  saveTasks();
  renderTasks();
  updateStats();

  input.value = '';
  document.getElementById('dueDateInput').value = '';
  input.focus();
}

// ── Enter key ─────────────────────────────────────────
document.getElementById('taskInput').addEventListener('keydown', e => {
  if (e.key === 'Enter') addTask();
});

// ── Toggle Complete ───────────────────────────────────
function toggleTask(id) {
  const t = tasks.find(t => t.id === id);
  if (t) { t.completed = !t.completed; saveTasks(); renderTasks(); updateStats(); }
}

// ── Delete Task ───────────────────────────────────────
function deleteTask(id) {
  const item = document.querySelector(`[data-id="${id}"]`);
  if (item) {
    item.style.transition = 'opacity .2s, transform .2s';
    item.style.opacity = '0';
    item.style.transform = 'translateX(20px)';
    setTimeout(() => {
      tasks = tasks.filter(t => t.id !== id);
      saveTasks(); renderTasks(); updateStats();
    }, 200);
  }
}

// ── Edit Task ─────────────────────────────────────────
function startEdit(id) {
  const t    = tasks.find(t => t.id === id);
  const item = document.querySelector(`[data-id="${id}"]`);
  if (!t || !item) return;

  const textEl = item.querySelector('.task-text');
  const input  = document.createElement('input');
  input.className   = 'task-edit-input';
  input.value       = t.text;
  textEl.replaceWith(input);
  input.focus();
  input.select();

  const editBtn = item.querySelector('.edit-btn');
  editBtn.textContent = '✓';
  editBtn.onclick = () => finishEdit(id, input.value);

  input.addEventListener('keydown', e => {
    if (e.key === 'Enter')  finishEdit(id, input.value);
    if (e.key === 'Escape') { saveTasks(); renderTasks(); }
  });
}

function finishEdit(id, newText) {
  const t = tasks.find(t => t.id === id);
  if (t && newText.trim()) {
    t.text = newText.trim();
    saveTasks(); renderTasks(); updateStats();
  }
}

// ── Clear Completed ───────────────────────────────────
function clearCompleted() {
  tasks = tasks.filter(t => !t.completed);
  saveTasks(); renderTasks(); updateStats();
}

// ── Filter ────────────────────────────────────────────
function setFilter(filter, btn) {
  currentFilter = filter;
  document.querySelectorAll('.filter-tab').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  renderTasks();
}

// ── Sort ──────────────────────────────────────────────
function sortedTasks(list) {
  const mode = document.getElementById('sortSelect').value;
  const priorityOrder = { high: 0, medium: 1, low: 2 };
  return [...list].sort((a, b) => {
    switch (mode) {
      case 'oldest':   return new Date(a.createdAt) - new Date(b.createdAt);
      case 'priority': return priorityOrder[a.priority] - priorityOrder[b.priority];
      case 'alpha':    return a.text.localeCompare(b.text);
      case 'duedate':
        if (!a.dueDate && !b.dueDate) return 0;
        if (!a.dueDate) return 1;
        if (!b.dueDate) return -1;
        return new Date(a.dueDate) - new Date(b.dueDate);
      default:         return new Date(b.createdAt) - new Date(a.createdAt);
    }
  });
}

// ── Render ────────────────────────────────────────────
function renderTasks() {
  const list       = document.getElementById('taskList');
  const empty      = document.getElementById('emptyState');
  const search     = document.getElementById('searchInput').value.toLowerCase();

  let filtered = tasks.filter(t => {
    if (currentFilter === 'active')    return !t.completed;
    if (currentFilter === 'completed') return t.completed;
    if (currentFilter === 'high')      return t.priority === 'high';
    return true;
  });

  if (search) filtered = filtered.filter(t => t.text.toLowerCase().includes(search));

  filtered = sortedTasks(filtered);

  list.innerHTML = '';

  if (!filtered.length) {
    empty.style.display = 'block';
    return;
  }
  empty.style.display = 'none';

  filtered.forEach(t => {
    const dueStatus = getDueStatus(t.dueDate);
    const dueLabel  = t.dueDate
      ? `<span class="task-due ${dueStatus || ''}">${
          dueStatus === 'overdue' ? '⚠ Overdue · ' :
          dueStatus === 'today'   ? '⏰ Today · '   : '📅 '
        }${formatDate(t.dueDate)}</span>`
      : '';

    const li = document.createElement('li');
    li.className   = `task-item priority-${t.priority}${t.completed ? ' completed' : ''}`;
    li.dataset.id  = t.id;
    li.innerHTML   = `
      <input type="checkbox" class="task-check" ${t.completed ? 'checked' : ''}
             onchange="toggleTask('${t.id}')"/>
      <div class="task-body">
        <div class="task-text">${escapeHTML(t.text)}</div>
        <div class="task-meta">
          <span class="tag tag-${t.priority}">${t.priority}</span>
          <span class="task-date">${new Date(t.createdAt).toLocaleDateString('en-IN',{day:'2-digit',month:'short'})}</span>
          ${dueLabel}
        </div>
      </div>
      <div class="task-actions">
        <button class="task-btn edit-btn" onclick="startEdit('${t.id}')" title="Edit">✎</button>
        <button class="task-btn del-btn"  onclick="deleteTask('${t.id}')" title="Delete">✕</button>
      </div>`;
    list.appendChild(li);
  });
}

// ── Stats ─────────────────────────────────────────────
function updateStats() {
  const total = tasks.length;
  const done  = tasks.filter(t => t.completed).length;
  const active = total - done;
  const pct   = total ? Math.round((done / total) * 100) : 0;

  document.getElementById('totalCount').textContent  = total;
  document.getElementById('activeCount').textContent = active;
  document.getElementById('doneCount').textContent   = done;
  document.getElementById('progressFill').style.width = pct + '%';
  document.getElementById('progressLabel').textContent = pct + '%';
}

// ── Export ────────────────────────────────────────────
function exportTasks() {
  const blob = new Blob([JSON.stringify(tasks, null, 2)], { type: 'application/json' });
  const url  = URL.createObjectURL(blob);
  const a    = document.createElement('a');
  a.href     = url;
  a.download = 'tasks_ex03.json';
  a.click();
  URL.revokeObjectURL(url);
}

// ── Init ──────────────────────────────────────────────
renderTasks();
updateStats();
```
## OUTPUT
<img width="2505" height="1295" alt="image" src="https://github.com/user-attachments/assets/f0ebaaa2-80d8-49fd-ba98-380576646b62" />


## RESULT
The program for creating To-do list using JavaScript is executed successfully.
