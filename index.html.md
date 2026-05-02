# index.html  
  
<!DOCTYPE html>  
<html lang="ar" dir="rtl">  
<head>  
<meta charset="UTF-8">  
<title>Study Space Pro</title>  
  
<style>  
body {  
  font-family: Arial;  
  margin: 0;  
  background: linear-gradient(135deg, #4a6cf7, #9f7aea);  
  color: #333;  
}  
  
header {  
  color: white;  
  text-align: center;  
  padding: 20px;  
}  
  
.container {  
  padding: 20px;  
}  
  
.box {  
  background: white;  
  padding: 15px;  
  margin-bottom: 15px;  
  border-radius: 12px;  
}  
  
input, button {  
  padding: 10px;  
  margin-top: 10px;  
  border-radius: 6px;  
  border: none;  
}  
  
button {  
  background: #4a6cf7;  
  color: white;  
  cursor: pointer;  
}  
  
.task {  
  display: flex;  
  justify-content: space-between;  
  margin-top: 10px;  
}  
.done {  
  text-decoration: line-through;  
  color: gray;  
}  
</style>  
  
</head>  
  
<body>  
  
<header>  
<h1>📚 Study Space Pro</h1>  
<p>نظم حياتك الدراسية بذكاء</p>  
</header>  
  
<div class="container">  
  
<!-- تسجيل دخول -->  
<div class="box" id="loginBox">  
<h3>تسجيل الدخول</h3>  
<input id="username" placeholder="اكتب اسمك">  
<button onclick="login()">دخول</button>  
</div>  
  
<!-- التطبيق -->  
<div id="app" style="display:none;">  
  
<div class="box">  
<h3 id="welcome"></h3>  
</div>  
  
<!-- المهام -->  
<div class="box">  
<h3>المهام</h3>  
<input id="taskInput" placeholder="مهمة جديدة">  
<button onclick="addTask()">إضافة</button>  
<div id="taskList"></div>  
</div>  
  
<!-- الأهداف -->  
<div class="box">  
<h3>الأهداف</h3>  
<input id="goalInput" placeholder="هدف جديد">  
<button onclick="addGoal()">إضافة</button>  
<div id="goalList"></div>  
</div>  
  
<!-- المؤقت -->  
<div class="box">  
<h3>مؤقت الدراسة ⏱️</h3>  
<p id="timer">25:00</p>  
<button onclick="startTimer()">ابدأ</button>  
</div>  
  
<!-- الجدول -->  
<div class="box">  
<h3>جدولك</h3>  
<textarea id="schedule" placeholder="اكتب جدولك هنا..." style="width:100%;height:100px;"></textarea>  
</div>  
  
</div>  
  
</div>  
  
<script>  
  
// تسجيل دخول  
function login(){  
 let name = document.getElementById("username").value;  
 localStorage.setItem("user", name);  
 loadApp();  
}  
  
function loadApp(){  
 let user = localStorage.getItem("user");  
 if(user){  
  document.getElementById("loginBox").style.display="none";  
  document.getElementById("app").style.display="block";  
  document.getElementById("welcome").innerText = "أهلًا " + user;  
 }  
}  
  
loadApp();  
  
// مهام  
function addTask(){  
 let val = taskInput.value;  
 let tasks = JSON.parse(localStorage.getItem("tasks")||"[]");  
 tasks.push({text:val,done:false});  
 localStorage.setItem("tasks", JSON.stringify(tasks));  
 renderTasks();  
 taskInput.value="";  
}  
  
function renderTasks(){  
 let tasks = JSON.parse(localStorage.getItem("tasks")||"[]");  
 taskList.innerHTML="";  
 tasks.forEach((t,i)=>{  
  let div=document.createElement("div");  
  div.className="task"+(t.done?" done":"");  
  div.innerHTML = t.text +   
  " <div><button onclick='toggleTask("+i+")'>✔️</button> <button onclick='deleteTask("+i+")'>❌</button></div>";  
  taskList.appendChild(div);  
 });  
}  
  
function toggleTask(i){  
 let tasks = JSON.parse(localStorage.getItem("tasks"));  
 tasks[i].done=!tasks[i].done;  
 localStorage.setItem("tasks", JSON.stringify(tasks));  
 renderTasks();  
}  
  
function deleteTask(i){  
 let tasks = JSON.parse(localStorage.getItem("tasks"));  
 tasks.splice(i,1);  
 localStorage.setItem("tasks", JSON.stringify(tasks));  
 renderTasks();  
}  
  
// أهداف  
function addGoal(){  
 let val = goalInput.value;  
 let goals = JSON.parse(localStorage.getItem("goals")||"[]");  
 goals.push(val);  
 localStorage.setItem("goals", JSON.stringify(goals));  
 renderGoals();  
 goalInput.value="";  
}  
  
function renderGoals(){  
 let goals = JSON.parse(localStorage.getItem("goals")||"[]");  
 goalList.innerHTML="";  
 goals.forEach((g,i)=>{  
  let div=document.createElement("div");  
  div.className="task";  
  div.innerHTML = g + " <button onclick='deleteGoal("+i+")'>❌</button>";  
  goalList.appendChild(div);  
 });  
}  
  
function deleteGoal(i){  
 let goals = JSON.parse(localStorage.getItem("goals"));  
 goals.splice(i,1);  
 localStorage.setItem("goals", JSON.stringify(goals));  
 renderGoals();  
}  
  
// مؤقت  
let time=1500;  
function startTimer(){  
 let interval=setInterval(()=>{  
  let m=Math.floor(time/60);  
  let s=time%60;  
  timer. innerText = m+":"+(s<10?"0"+s:s);  
  time--;  
  if(time<0){clearInterval(interval);}  
 },1000);  
}  
  
// جدول حفظ  
schedule.value = localStorage.getItem("schedule")||"";  
schedule.oninput = ()=> localStorage.setItem("schedule", schedule.value);  
  
// تحميل البيانات  
renderTasks();  
renderGoals();  
  
</script>  
  
</body>  
</html>  
