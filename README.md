<!DOCTYPE html>
<html lang="ku">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pro To-Do</title>

<style>
body {
  font-family: Arial;
  background: linear-gradient(135deg, #3b82f6, #9333ea);
  margin: 0;
  color: white;
}

.container {
  max-width: 400px;
  margin: 40px auto;
  background: #111827;
  padding: 20px;
  border-radius: 20px;
}

input {
  padding: 10px;
  margin: 5px;
  width: 90%;
  border-radius: 10px;
  border: none;
}

button {
  padding: 10px;
  border: none;
  border-radius: 10px;
  background: #3b82f6;
  color: white;
}

.section {
  margin-top: 20px;
}

li {
  margin: 10px 0;
  padding: 10px;
  background: #1f2937;
  border-radius: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.done {
  text-decoration: line-through;
  color: gray;
}

.time {
  font-size: 12px;
  color: #94a3b8;
}
</style>
</head>

<body>

<div class="container">
  <h2>📝 My Tasks</h2>

  <input type="text" id="taskInput" placeholder="کار بنووسە...">
  <input type="time" id="timeInput">
  <button onclick="addTask()">➕ زیادکردن</button>

  <div class="section">
    <h3>📌 کارەکان</h3>
    <ul id="list"></ul>
  </div>

  <div class="section">
    <h3>✔️ تەواوبووەکان</h3>
    <ul id="doneList"></ul>
  </div>
</div>

<script>
let tasks = [];

function saveTasks() {
  localStorage.setItem("tasks", JSON.stringify(tasks));
}

function loadTasks() {
  let saved = localStorage.getItem("tasks");
  if (saved) {
    tasks = JSON.parse(saved);
    tasks.forEach(task => renderTask(task));
  }
}

function addTask() {
  let text = taskInput.value;
  let time = timeInput.value;

  if (text === "") return;

  let task = { text, time, done: false };
  tasks.push(task);
  saveTasks();
  renderTask(task);

  taskInput.value = "";
  timeInput.value = "";
}

function renderTask(task) {
  let li = document.createElement("li");

  let span = document.createElement("span");
  span.innerHTML = task.text + " <span class='time'>(" + task.time + ")</span>";

  if (task.done) span.classList.add("done");

  let check = document.createElement("button");
  check.innerHTML = "✔";

  check.onclick = () => {
    task.done = !task.done;
    saveTasks();
    refresh();
  };

  let del = document.createElement("button");
  del.innerHTML = "❌";

  del.onclick = () => {
    tasks = tasks.filter(t => t !== task);
    saveTasks();
    refresh();
  };

  li.appendChild(check);
  li.appendChild(span);
  li.appendChild(del);

  if (task.done) {
    document.getElementById("doneList").appendChild(li);
  } else {
    document.getElementById("list").appendChild(li);
  }
}

function refresh() {
  list.innerHTML = "";
  doneList.innerHTML = "";
  tasks.forEach(task => renderTask(task));
}

function checkTime() {
  let now = new Date();
  let current = now.getHours().toString().padStart(2,'0') + ":" +
                now.getMinutes().toString().padStart(2,'0');

  tasks.forEach(task => {
    if (task.time === current && !task.done) {
      alert("⏰ کاتە بۆ: " + task.text);
    }
  });
}

setInterval(checkTime, 60000);

loadTasks();
</script>

</body>
</html>
