
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Daily Routine</title>
<style>
  :root{
    box-sizing:border-box;
    padding-top:env(safe-area-inset-top,0px);
    padding-bottom:env(safe-area-inset-bottom,0px);
    --bg:#12002b;
    --bg2:#1d0040;
    --fg:#ffffff;
    --cyan:#00f0ff;
    --magenta:#ff2fd0;
    --yellow:#ffe600;
    --green:#3dff7a;
    --line:var(--cyan);
    --done-bg:#2a0050;
    --scale:1;
  }
  *,*::before,*::after{box-sizing:inherit}
  html{scroll-padding-top:env(safe-area-inset-top,0px)}
  body{
    margin:0;color:var(--fg);
    background:
      repeating-linear-gradient(0deg, rgba(255,47,208,.07) 0 2px, transparent 2px 5px),
      linear-gradient(180deg, #1d0040 0%, #12002b 55%, #23003f 100%);
    background-attachment:fixed;
    font-family:Verdana,"DejaVu Sans",Tahoma,sans-serif;font-weight:700;
    font-size:calc(18px * var(--scale));line-height:1.35;-webkit-text-size-adjust:100%;
  }
  .wrap{max-width:900px;margin:0 auto;padding:0 14px 130px}

  header{
    position:sticky;top:0;z-index:5;
    background:linear-gradient(180deg,#26004d 0%,#12002b 100%);
    padding-top:calc(env(safe-area-inset-top,0px) + 10px);
    border-bottom:5px solid var(--magenta);
    box-shadow:0 6px 18px rgba(255,47,208,.35);
  }
  .toprow{display:flex;gap:8px;align-items:center;margin-bottom:8px}
  select,input[type=text],input[type=number]{
    font:inherit;font-size:calc(16px * var(--scale));
    background:#0d0020;color:var(--yellow);border:3px solid var(--cyan);
    border-radius:6px;padding:9px 8px;width:100%;
  }
  h1{
    font-size:calc(21px * var(--scale));margin:0 0 4px;text-align:center;
    color:var(--yellow);letter-spacing:1px;
    text-shadow:0 0 8px rgba(255,230,0,.8), 2px 2px 0 var(--magenta);
  }
  .date{font-size:calc(15px * var(--scale));text-align:center;margin:0 0 10px;color:var(--cyan)}
  .days{display:flex;gap:6px;overflow-x:auto;padding-bottom:10px}
  .day{
    flex:0 0 auto;min-width:64px;padding:10px 6px;text-align:center;
    border:3px solid var(--cyan);background:#0d0020;color:var(--cyan);
    font:inherit;font-size:calc(15px * var(--scale));cursor:pointer;border-radius:6px;
  }
  .day[aria-pressed="true"]{
    background:var(--magenta);color:#12002b;border-color:var(--yellow);
    box-shadow:0 0 12px rgba(255,47,208,.9);
  }

  .next{
    border:5px solid var(--yellow);border-radius:10px;padding:14px;margin:16px 0 8px;
    background:linear-gradient(135deg,#2a0050,#3d0066);
    box-shadow:0 0 16px rgba(255,230,0,.45);
  }
  .next .label{font-size:calc(14px * var(--scale));margin-bottom:6px;color:var(--cyan);letter-spacing:1px}
  .next .task{font-size:calc(24px * var(--scale));line-height:1.25;color:var(--yellow)}
  .progress{font-size:calc(15px * var(--scale));margin:12px 0 6px;text-align:center;color:var(--green)}

  ul{list-style:none;margin:0;padding:0}
  li{border-bottom:3px solid rgba(0,240,255,.55);background:rgba(13,0,32,.55)}
  li:first-child{border-top:3px solid rgba(0,240,255,.55)}
  .row{
    display:flex;align-items:center;gap:12px;width:100%;
    padding:16px 8px;background:transparent;border:0;color:inherit;
    font:inherit;text-align:left;cursor:pointer;
  }
  .row .num{
    min-width:1.6em;font-size:calc(20px * var(--scale));
    color:var(--yellow);letter-spacing:1px;
    text-shadow:0 0 8px rgba(255,230,0,.8), 2px 2px 0 var(--magenta);
  }
  .row .name{
    flex:1;font-size:calc(21px * var(--scale));line-height:1.3;
    color:var(--yellow);letter-spacing:1px;
    text-shadow:0 0 8px rgba(255,230,0,.8), 2px 2px 0 var(--magenta);
  }
  .row .box{
    flex:0 0 auto;width:calc(40px * var(--scale));height:calc(40px * var(--scale));
    border:4px solid var(--yellow);border-radius:4px;display:flex;align-items:center;
    justify-content:center;font-size:calc(28px * var(--scale));line-height:1;
    color:var(--green);background:#0d0020;
  }
  li.done{background:var(--done-bg)}
  li.done .name,li.done .num{
    text-decoration:line-through;color:var(--cyan);
    text-shadow:0 0 6px rgba(0,240,255,.6);
  }
  li.done .num{text-decoration:none}
  li.done .box{border-color:var(--green);box-shadow:0 0 10px rgba(61,255,122,.7)}

  .edit-tools{display:flex;gap:6px;padding:0 8px 12px;flex-wrap:wrap}
  .edit-tools button{
    font:inherit;font-size:calc(15px * var(--scale));padding:8px 12px;
    border:3px solid var(--magenta);background:#0d0020;color:var(--magenta);
    border-radius:6px;cursor:pointer;
  }
  .daypicker{display:flex;gap:6px;flex-wrap:wrap;margin:10px 0 4px}
  .daypicker button{
    font:inherit;font-size:calc(14px * var(--scale));padding:8px 10px;
    border:3px solid var(--cyan);background:#0d0020;color:var(--cyan);
    border-radius:6px;cursor:pointer;
  }
  .daypicker button[aria-pressed="true"]{background:var(--cyan);color:#12002b}

  h2{
    font-size:calc(18px * var(--scale));margin:28px 0 8px;padding-bottom:6px;
    color:var(--magenta);letter-spacing:1px;
    border-bottom:5px solid var(--magenta);
    text-shadow:0 0 8px rgba(255,47,208,.7);
  }

  .timer{
    border:5px solid var(--cyan);border-radius:10px;padding:14px;margin:0 0 14px;
    background:linear-gradient(135deg,#1a0038,#2a0050);
    box-shadow:0 0 14px rgba(0,240,255,.35);
  }
  .timer .tname{font-size:calc(19px * var(--scale));margin-bottom:2px;color:var(--cyan)}
  .timer .clock{
    font-size:calc(38px * var(--scale));line-height:1.1;letter-spacing:2px;margin:6px 0 12px;
    color:var(--yellow);text-shadow:0 0 10px rgba(255,230,0,.7);
  }
  .timer.ring{
    background:var(--magenta);border-color:var(--yellow);
    animation:flash 1s steps(2,end) infinite;
  }
  .timer.ring .tname,.timer.ring .clock{color:#12002b;text-shadow:none}
  .timer.ring button{border-color:#12002b;color:#12002b;background:var(--yellow)}
  @keyframes flash{50%{background:var(--yellow)}}
  .tbtns{display:flex;gap:10px;flex-wrap:wrap}
  .tbtns button{
    font:inherit;font-size:calc(16px * var(--scale));padding:12px 18px;
    border:3px solid var(--green);background:#0d0020;color:var(--green);
    border-radius:6px;cursor:pointer;
  }

  .panel{
    border:5px solid var(--green);border-radius:10px;padding:14px;margin:14px 0;
    background:rgba(13,0,32,.75);
  }
  .panel h3{font-size:calc(17px * var(--scale));margin:0 0 10px;color:var(--green);letter-spacing:1px}
  .prow{display:flex;gap:8px;margin-bottom:10px;flex-wrap:wrap}
  .panel button{
    font:inherit;font-size:calc(16px * var(--scale));padding:11px 14px;
    border:3px solid var(--yellow);background:#0d0020;color:var(--yellow);
    border-radius:6px;cursor:pointer;
  }
  #msg{color:var(--magenta)}
  .hidden{display:none}
  .inline-edit{display:flex;gap:8px;flex-wrap:wrap;align-items:center;padding:12px 8px}
  .inline-edit input{flex:1;min-width:150px}
  .inline-edit button{
    font:inherit;font-size:calc(15px * var(--scale));padding:10px 14px;
    border:3px solid var(--yellow);background:#0d0020;color:var(--yellow);
    border-radius:6px;cursor:pointer;
  }

  .bar{
    position:fixed;left:0;right:0;bottom:0;z-index:6;
    display:flex;gap:8px;justify-content:center;flex-wrap:wrap;
    background:linear-gradient(0deg,#26004d,#12002b);
    border-top:5px solid var(--cyan);
    box-shadow:0 -6px 18px rgba(0,240,255,.3);
    padding:10px 10px calc(10px + env(safe-area-inset-bottom,0px));
  }
  .bar button{
    font:inherit;font-size:calc(15px * var(--scale));padding:11px 13px;
    border:3px solid var(--magenta);background:#0d0020;color:var(--magenta);
    border-radius:6px;cursor:pointer;
  }
  .bar button[aria-pressed="true"]{
    background:var(--magenta);color:#12002b;border-color:var(--yellow);
    box-shadow:0 0 12px rgba(255,47,208,.9);
  }
</style>
</head>
<body>
<div class="wrap">
  <header>
    <div class="toprow">
      <select id="profileSelect" aria-label="Profile"></select>
      <button class="day" id="profilesBtn" type="button" style="min-width:auto">&#9881;</button>
    </div>
    <h1 id="title">DAILY ROUTINE</h1>
    <p class="date" id="today"></p>
    <div class="days" id="days"></div>
  </header>

  <div class="panel hidden" id="profilePanel">
    <h3>PROFILES</h3>
    <input type="text" id="profileNameInput" placeholder="Profile name">
    <div class="prow" style="margin-top:10px">
      <button id="newBlank" type="button">New (blank)</button>
      <button id="newCopy" type="button">New (copy this one)</button>
    </div>
    <div class="prow">
      <button id="renameProfile" type="button">Rename to this</button>
      <button id="deleteProfile" type="button">Delete this profile</button>
    </div>
    <p id="msg" style="margin:6px 0 0;font-size:calc(15px * var(--scale))"></p>
  </div>

  <div class="next" id="next">
    <div class="label">DO THIS NEXT:</div>
    <div class="task" id="nextTask">&mdash;</div>
  </div>

  <p class="progress" id="progress"></p>

  <ul id="list"></ul>

  <div class="panel hidden" id="addPanel">
    <h3>ADD A TASK</h3>
    <input type="text" id="newTaskName" placeholder="Task name">
    <div class="daypicker" id="newTaskDays"></div>
    <div class="prow" style="margin-top:8px">
      <button id="addTaskBtn" type="button">Add task</button>
    </div>
  </div>

  <h2 id="stepsHeading">STEP LIST</h2>
  <ul id="stepList"></ul>
  <p class="progress" id="stepProgress"></p>
  <div class="panel hidden" id="addStepPanel">
    <h3>ADD A STEP</h3>
    <input type="text" id="newStepName" placeholder="Step">
    <div class="prow" style="margin-top:8px">
      <button id="addStepBtn" type="button">Add step</button>
    </div>
  </div>

  <h2>TIMERS</h2>
  <div id="timers"></div>
  <div class="panel hidden" id="addTimerPanel">
    <h3>ADD A TIMER</h3>
    <input type="text" id="newTimerName" placeholder="Timer name">
    <div style="height:8px"></div>
    <input type="number" id="newTimerMins" placeholder="Minutes" min="1" step="1" value="120">
    <div class="prow" style="margin-top:8px">
      <button id="addTimerBtn" type="button">Add timer</button>
    </div>
  </div>
</div>

<div class="bar">
  <button id="smaller" type="button">A&minus;</button>
  <button id="bigger" type="button">A+</button>
  <button id="editBtn" type="button" aria-pressed="false">Edit</button>
  <button id="reset" type="button">Start over</button>
</div>

<script>
(function(){
  var DAY_CODES = ["SUN","MON","TUE","WED","THU","FRI","SAT"];
  var WEEK = ["MON","TUE","WED","THU","FRI","SAT","SUN"];
  var FULL = {SUN:"Sunday",MON:"Monday",TUE:"Tuesday",WED:"Wednesday",THU:"Thursday",FRI:"Friday",SAT:"Saturday"};
  var PROFILES_KEY = "routine:profiles";

  var editMode = false, scale = 1;
  var store = null, marks = {}, timerState = {}, rang = {};
  var newTaskDays = {};

  function uid(){ return Math.random().toString(36).slice(2,9); }

  function tannerProfile(){
    var ORDER = ["blood","dress","laundry","exercise","meds","trash","bigtrash","pool","dust","vacuum","bathroom","mealprep","water","evening"];
    var L = {
      blood:"Blood sugar / BP", dress:"Dress / coffee", laundry:"Start laundry",
      exercise:"Exercise", meds:"Meds \u2014 med boxes", trash:"Trash \u2014 kitchen check",
      bigtrash:"Big trash out", pool:"Pool", dust:"Dust bedroom", vacuum:"Vacuum",
      bathroom:"Bathroom", mealprep:"Meal prep", water:"Water plants",
      evening:"Evening check \u2014 trash in room"
    };
    var EVERY = ["blood","dress","exercise","pool","evening"];
    var EXTRA = {
      MON:["laundry","meds","trash","dust","vacuum","bathroom","mealprep"],
      TUE:["bigtrash","water"],
      WED:["trash","vacuum"],
      THU:["water"],
      FRI:["laundry","meds","trash","vacuum"],
      SAT:["water"],
      SUN:["trash","vacuum"]
    };
    var OV = { MON:{vacuum:"Vacuum bedroom"}, FRI:{laundry:"Start laundry \u2014 bedding (every other week)"} };
    var days = {};
    WEEK.forEach(function(d){
      var keys = {};
      EVERY.concat(EXTRA[d] || []).forEach(function(k){ keys[k] = true; });
      var ov = OV[d] || {};
      days[d] = ORDER.filter(function(k){ return keys[k]; }).map(function(k){
        return { id: uid(), name: ov[k] || L[k] };
      });
    });
    return {
      id: uid(), name: "Tanner",
      work: { TUE:true, WED:true, THU:true },
      stepsTitle: "POOL \u2014 INCLUDES",
      days: days,
      steps: [
        "Empty basket",
        "Clear leaves from pool \u2014 all leaves, not just a clump",
        "Put Louie in",
        "Make sure Huey is empty and in pool",
        "Start pool pumps",
        "Start waterfall",
        "Take Louie out when he is done & clean him",
        "Shut off waterfall, then pump (set timer)"
      ].map(function(s){ return { id: uid(), name: s }; }),
      timers: [
        { id: uid(), name:"LOUIE", minutes:120, doneText:"TAKE LOUIE OUT" },
        { id: uid(), name:"POOL PUMP", minutes:120, doneText:"SHUT OFF PUMP" }
      ]
    };
  }

  function blankProfile(name){
    var days = {};
    WEEK.forEach(function(d){ days[d] = []; });
    return { id: uid(), name: name || "New profile", work: {}, stepsTitle: "STEP LIST", days: days, steps: [], timers: [] };
  }

  function loadStore(){
    try {
      var raw = localStorage.getItem(PROFILES_KEY);
      store = raw ? JSON.parse(raw) : null;
    } catch(e){ store = null; }
    if (!store || !store.profiles || !store.profiles.length){
      var t = tannerProfile();
      store = { profiles: [t], activeId: t.id };
      saveStore();
    }
    if (!activeProfile()) store.activeId = store.profiles[0].id;
  }
  function saveStore(){
    try { localStorage.setItem(PROFILES_KEY, JSON.stringify(store)); } catch(e){}
  }
  function activeProfile(){
    for (var i=0;i<store.profiles.length;i++){
      if (store.profiles[i].id === store.activeId) return store.profiles[i];
    }
    return null;
  }

  var now = new Date();
  var todayCode = DAY_CODES[now.getDay()];
  var dateStamp = now.getFullYear() + "-" + (now.getMonth()+1) + "-" + now.getDate();
  var selected = todayCode;

  function marksKey(){ return "routine:marks:" + store.activeId + ":" + dateStamp; }
  function loadMarks(){
    try {
      var raw = localStorage.getItem(marksKey());
      marks = raw ? JSON.parse(raw) : {};
    } catch(e){ marks = {}; }
    if (!marks || typeof marks !== "object") marks = {};
  }
  function saveMarks(){
    try { localStorage.setItem(marksKey(), JSON.stringify(marks)); } catch(e){}
  }

  function timersKey(){ return "routine:timers:" + store.activeId; }
  function loadTimers(){
    try {
      var raw = localStorage.getItem(timersKey());
      timerState = raw ? JSON.parse(raw) : {};
    } catch(e){ timerState = {}; }
    if (!timerState || typeof timerState !== "object") timerState = {};
    rang = {};
  }
  function saveTimers(){
    try { localStorage.setItem(timersKey(), JSON.stringify(timerState)); } catch(e){}
  }

  function move(arr, i, delta){
    var j = i + delta;
    if (j < 0 || j >= arr.length) return;
    var tmp = arr[i]; arr[i] = arr[j]; arr[j] = tmp;
  }

  var renaming = null;   // {kind:"task"|"step", id:"..."}
  var editingTimer = null;

  function mkBtn(label, fn){
    var b = document.createElement("button");
    b.type = "button"; b.textContent = label;
    b.addEventListener("click", fn);
    return b;
  }

  function mkRemoveBtn(onRemove){
    var armed = false, timer = null;
    var b = document.createElement("button");
    b.type = "button"; b.textContent = "Remove";
    b.addEventListener("click", function(){
      if (!armed){
        armed = true;
        b.textContent = "Tap again to remove";
        timer = setTimeout(function(){ armed = false; b.textContent = "Remove"; }, 4000);
      } else {
        clearTimeout(timer);
        onRemove();
      }
    });
    return b;
  }

  function editTools(onUp, onDown, onRename, onRemove){
    var box = document.createElement("div");
    box.className = "edit-tools";
    box.appendChild(mkBtn("\u2191", onUp));
    box.appendChild(mkBtn("\u2193", onDown));
    box.appendChild(mkBtn("Rename", onRename));
    box.appendChild(mkRemoveBtn(onRemove));
    return box;
  }

  function inlineEditor(value, onSave){
    var wrap = document.createElement("div");
    wrap.className = "inline-edit";
    var input = document.createElement("input");
    input.type = "text"; input.value = value;
    var save = mkBtn("Save", function(){
      var v = input.value.trim();
      if (v) onSave(v);
      renaming = null; render();
    });
    var cancel = mkBtn("Cancel", function(){ renaming = null; render(); });
    input.addEventListener("keydown", function(e){ if (e.key === "Enter") save.click(); });
    wrap.appendChild(input); wrap.appendChild(save); wrap.appendChild(cancel);
    setTimeout(function(){ try { input.focus(); } catch(e){} }, 0);
    return wrap;
  }

  function showMsg(text){
    var el = document.getElementById("msg");
    if (el){ el.textContent = text; setTimeout(function(){ if (el.textContent === text) el.textContent = ""; }, 4000); }
  }

  function makeRow(index, label, isDone, onToggle){
    var btn = document.createElement("button");
    btn.type = "button"; btn.className = "row";
    btn.setAttribute("aria-pressed", isDone ? "true" : "false");

    var num = document.createElement("span");
    num.className = "num"; num.textContent = (index+1) + ".";
    var name = document.createElement("span");
    name.className = "name"; name.textContent = label;
    var box = document.createElement("span");
    box.className = "box"; box.setAttribute("aria-hidden","true");
    box.textContent = isDone ? "\u2714" : "";

    btn.appendChild(num); btn.appendChild(name); btn.appendChild(box);
    btn.addEventListener("click", function(){ if (!editMode) onToggle(); });
    return btn;
  }

  function renderTasks(){
    var prof = activeProfile();
    if (!prof.days[selected]) prof.days[selected] = [];
    var tasks = prof.days[selected];
    var dayMarks = marks[selected] || {};
    var list = document.getElementById("list");
    list.innerHTML = "";
    var done = 0, nextName = null;

    tasks.forEach(function(task, i){
      var isDone = !!dayMarks[task.id];
      if (isDone) done++; else if (nextName === null) nextName = task.name;

      var li = document.createElement("li");
      if (isDone) li.className = "done";

      if (editMode && renaming && renaming.kind === "task" && renaming.id === task.id){
        li.appendChild(inlineEditor(task.name, function(v){ task.name = v; saveStore(); }));
      } else {
        li.appendChild(makeRow(i, task.name, isDone, function(){
          if (!marks[selected]) marks[selected] = {};
          if (marks[selected][task.id]) delete marks[selected][task.id];
          else marks[selected][task.id] = true;
          saveMarks(); render();
        }));
        if (editMode){
          li.appendChild(editTools(
            function(){ move(tasks, i, -1); saveStore(); render(); },
            function(){ move(tasks, i, 1); saveStore(); render(); },
            function(){ renaming = { kind:"task", id:task.id }; render(); },
            function(){ tasks.splice(i,1); saveStore(); render(); }
          ));
        }
      }
      list.appendChild(li);
    });

    document.getElementById("progress").textContent = done + " of " + tasks.length + " done";

    var nextBox = document.getElementById("next");
    var nextEl = document.getElementById("nextTask");
    if (!tasks.length){
      nextBox.querySelector(".label").textContent = "NOTHING ON THIS DAY";
      nextEl.textContent = "Tap Edit to add tasks.";
    } else if (nextName === null){
      nextBox.querySelector(".label").textContent = "FINISHED";
      nextEl.textContent = "All done for " + FULL[selected] + "!";
    } else {
      nextBox.querySelector(".label").textContent = "DO THIS NEXT:";
      nextEl.textContent = nextName;
    }
  }

  function renderSteps(){
    var prof = activeProfile();
    if (!prof.steps) prof.steps = [];
    document.getElementById("stepsHeading").textContent = prof.stepsTitle || "STEP LIST";
    var steps = prof.steps;
    var stepMarks = (marks.steps && marks.steps[selected]) ? marks.steps[selected] : {};
    var ul = document.getElementById("stepList");
    ul.innerHTML = "";
    var done = 0;

    steps.forEach(function(step, i){
      var isDone = !!stepMarks[step.id];
      if (isDone) done++;
      var li = document.createElement("li");
      if (isDone) li.className = "done";

      if (editMode && renaming && renaming.kind === "step" && renaming.id === step.id){
        li.appendChild(inlineEditor(step.name, function(v){ step.name = v; saveStore(); }));
      } else {
        li.appendChild(makeRow(i, step.name, isDone, function(){
          if (!marks.steps) marks.steps = {};
          if (!marks.steps[selected]) marks.steps[selected] = {};
          if (marks.steps[selected][step.id]) delete marks.steps[selected][step.id];
          else marks.steps[selected][step.id] = true;
          saveMarks(); render();
        }));
        if (editMode){
          li.appendChild(editTools(
            function(){ move(steps, i, -1); saveStore(); render(); },
            function(){ move(steps, i, 1); saveStore(); render(); },
            function(){ renaming = { kind:"step", id:step.id }; render(); },
            function(){ steps.splice(i,1); saveStore(); render(); }
          ));
        }
      }
      ul.appendChild(li);
    });

    document.getElementById("stepProgress").textContent =
      steps.length ? (done + " of " + steps.length + " steps done") : "";
  }

  function fmt(ms){
    if (ms < 0) ms = 0;
    var total = Math.ceil(ms/1000);
    var h = Math.floor(total/3600), m = Math.floor((total%3600)/60), s = total%60;
    return h + ":" + (m<10?"0":"") + m + ":" + (s<10?"0":"") + s;
  }

  function beep(){
    try {
      var Ctx = window.AudioContext || window.webkitAudioContext;
      if (!Ctx) return;
      var ctx = new Ctx();
      [0,700,1400].forEach(function(d){
        setTimeout(function(){
          var o = ctx.createOscillator(), g = ctx.createGain();
          o.type = "square"; o.frequency.value = 880; g.gain.value = 0.15;
          o.connect(g); g.connect(ctx.destination); o.start();
          setTimeout(function(){ o.stop(); }, 450);
        }, d);
      });
    } catch(e){}
  }

  function renderTimers(){
    var prof = activeProfile();
    if (!prof.timers) prof.timers = [];
    var wrap = document.getElementById("timers");
    wrap.innerHTML = "";

    prof.timers.forEach(function(tm, i){
      var card = document.createElement("div");
      card.className = "timer"; card.id = "timer-" + tm.id;

      var nameEl = document.createElement("div");
      nameEl.className = "tname";
      nameEl.textContent = tm.name + " \u2014 " + tm.minutes + " MIN";

      var clock = document.createElement("div");
      clock.className = "clock"; clock.id = "clock-" + tm.id;

      var btns = document.createElement("div");
      btns.className = "tbtns";

      var startBtn = document.createElement("button");
      startBtn.type = "button"; startBtn.id = "start-" + tm.id; startBtn.textContent = "START";
      startBtn.addEventListener("click", function(){
        timerState[tm.id] = Date.now(); rang[tm.id] = false; saveTimers(); tick();
      });

      var stopBtn = document.createElement("button");
      stopBtn.type = "button"; stopBtn.textContent = "STOP";
      stopBtn.addEventListener("click", function(){
        delete timerState[tm.id]; rang[tm.id] = false; saveTimers(); tick();
      });

      btns.appendChild(startBtn); btns.appendChild(stopBtn);

      if (editMode){
        btns.appendChild(mkBtn("Edit", function(){ editingTimer = tm.id; render(); }));
        btns.appendChild(mkRemoveBtn(function(){
          prof.timers.splice(i,1); delete timerState[tm.id];
          saveStore(); saveTimers(); render();
        }));
      }

      card.appendChild(nameEl); card.appendChild(clock); card.appendChild(btns);

      if (editMode && editingTimer === tm.id){
        var ed = document.createElement("div");
        ed.className = "inline-edit";
        var ni = document.createElement("input");
        ni.type = "text"; ni.value = tm.name;
        var mi = document.createElement("input");
        mi.type = "number"; mi.min = "1"; mi.value = String(tm.minutes);
        mi.style.maxWidth = "8em";
        ed.appendChild(ni); ed.appendChild(mi);
        ed.appendChild(mkBtn("Save", function(){
          var v = ni.value.trim();
          var mv = parseInt(mi.value, 10);
          if (v) tm.name = v;
          if (mv > 0) tm.minutes = mv;
          editingTimer = null; saveStore(); render();
        }));
        ed.appendChild(mkBtn("Cancel", function(){ editingTimer = null; render(); }));
        card.appendChild(ed);
      }

      wrap.appendChild(card);
    });

    tick();
  }

  function tick(){
    var prof = activeProfile();
    if (!prof || !prof.timers) return;
    prof.timers.forEach(function(tm){
      var card = document.getElementById("timer-" + tm.id);
      var clock = document.getElementById("clock-" + tm.id);
      var startBtn = document.getElementById("start-" + tm.id);
      if (!card || !clock) return;

      var total = (tm.minutes || 0) * 60000;
      var started = timerState[tm.id];
      if (!started){
        card.className = "timer";
        clock.textContent = fmt(total);
        if (startBtn) startBtn.textContent = "START";
        return;
      }
      var left = started + total - Date.now();
      if (startBtn) startBtn.textContent = "RESTART";
      if (left <= 0){
        card.className = "timer ring";
        clock.textContent = tm.doneText || "TIME'S UP";
        if (!rang[tm.id]){ rang[tm.id] = true; beep(); }
      } else {
        card.className = "timer";
        clock.textContent = fmt(left);
      }
    });
  }

  function renderDays(){
    var prof = activeProfile();
    var wrap = document.getElementById("days");
    wrap.innerHTML = "";
    WEEK.forEach(function(d){
      var b = document.createElement("button");
      b.type = "button"; b.className = "day"; b.dataset.day = d;
      b.textContent = d;
      if (prof.work && prof.work[d]){
        var small = document.createElement("div");
        small.style.fontSize = "0.75em"; small.textContent = "8\u20134";
        b.appendChild(small);
      }
      b.setAttribute("aria-pressed", d === selected ? "true" : "false");
      b.addEventListener("click", function(){ selected = d; render(); });
      wrap.appendChild(b);
    });
  }

  function renderProfileSelect(){
    var sel = document.getElementById("profileSelect");
    sel.innerHTML = "";
    store.profiles.forEach(function(pr){
      var o = document.createElement("option");
      o.value = pr.id; o.textContent = pr.name;
      if (pr.id === store.activeId) o.selected = true;
      sel.appendChild(o);
    });
    document.getElementById("title").textContent =
      (activeProfile().name || "DAILY").toUpperCase() + " \u2014 DAILY ROUTINE";
  }

  function renderDayPicker(){
    var wrap = document.getElementById("newTaskDays");
    wrap.innerHTML = "";
    WEEK.forEach(function(d){
      var b = document.createElement("button");
      b.type = "button"; b.textContent = d;
      b.setAttribute("aria-pressed", newTaskDays[d] ? "true" : "false");
      b.addEventListener("click", function(){
        newTaskDays[d] = !newTaskDays[d];
        b.setAttribute("aria-pressed", newTaskDays[d] ? "true" : "false");
      });
      wrap.appendChild(b);
    });
  }

  function render(){
    renderProfileSelect();
    renderDays();
    renderTasks();
    renderSteps();
    renderTimers();
    document.getElementById("addPanel").className = editMode ? "panel" : "panel hidden";
    document.getElementById("addStepPanel").className = editMode ? "panel" : "panel hidden";
    document.getElementById("addTimerPanel").className = editMode ? "panel" : "panel hidden";
    document.getElementById("editBtn").setAttribute("aria-pressed", editMode ? "true" : "false");
  }

  document.getElementById("profileSelect").addEventListener("change", function(e){
    store.activeId = e.target.value; saveStore();
    loadMarks(); loadTimers(); render();
  });

  document.getElementById("profilesBtn").addEventListener("click", function(){
    var p = document.getElementById("profilePanel");
    p.className = p.className.indexOf("hidden") >= 0 ? "panel" : "panel hidden";
  });

  function profileNameField(){
    return document.getElementById("profileNameInput");
  }

  document.getElementById("newBlank").addEventListener("click", function(){
    var f = profileNameField();
    var n = f.value.trim();
    if (!n){ showMsg("Type a name in the box first."); return; }
    f.value = "";
    var pr = blankProfile(n);
    store.profiles.push(pr); store.activeId = pr.id; saveStore();
    loadMarks(); loadTimers(); render();
  });

  document.getElementById("newCopy").addEventListener("click", function(){
    var src = activeProfile();
    var f = profileNameField();
    var n = f.value.trim();
    if (!n){ showMsg("Type a name for the copy first."); return; }
    f.value = "";
    var copy = JSON.parse(JSON.stringify(src));
    copy.id = uid(); copy.name = n;
    WEEK.forEach(function(d){
      copy.days[d] = (copy.days[d] || []).map(function(t){ return { id: uid(), name: t.name }; });
    });
    copy.steps = (copy.steps || []).map(function(s){ return { id: uid(), name: s.name }; });
    copy.timers = (copy.timers || []).map(function(t){
      return { id: uid(), name: t.name, minutes: t.minutes, doneText: t.doneText };
    });
    store.profiles.push(copy); store.activeId = copy.id; saveStore();
    loadMarks(); loadTimers(); render();
  });

  document.getElementById("renameProfile").addEventListener("click", function(){
    var pr = activeProfile();
    var f = profileNameField();
    var n = f.value.trim();
    if (!n){ showMsg("Type the new name in the box first."); return; }
    pr.name = n; f.value = "";
    saveStore(); render(); showMsg("Renamed.");
  });

  (function(){
    var btn = document.getElementById("deleteProfile");
    var armed = false, t = null;
    btn.addEventListener("click", function(){
      if (store.profiles.length < 2){ showMsg("Keep at least one profile."); return; }
      if (!armed){
        armed = true;
        btn.textContent = "Tap again to delete";
        t = setTimeout(function(){ armed = false; btn.textContent = "Delete this profile"; }, 4000);
        return;
      }
      clearTimeout(t); armed = false; btn.textContent = "Delete this profile";
      var pr = activeProfile();
      store.profiles = store.profiles.filter(function(x){ return x.id !== pr.id; });
      store.activeId = store.profiles[0].id;
      saveStore(); loadMarks(); loadTimers(); render();
    });
  })();

  document.getElementById("addTaskBtn").addEventListener("click", function(){
    var input = document.getElementById("newTaskName");
    var name = input.value.trim();
    if (!name){ showMsg("Type a task name first."); return; }
    var prof = activeProfile();
    var any = false;
    WEEK.forEach(function(d){
      if (newTaskDays[d]){
        any = true;
        prof.days[d] = prof.days[d] || [];
        prof.days[d].push({ id: uid(), name: name });
      }
    });
    if (!any){
      prof.days[selected] = prof.days[selected] || [];
      prof.days[selected].push({ id: uid(), name: name });
    }
    input.value = "";
    newTaskDays = {}; renderDayPicker();
    saveStore(); render();
  });

  document.getElementById("addStepBtn").addEventListener("click", function(){
    var input = document.getElementById("newStepName");
    var name = input.value.trim();
    if (!name){ showMsg("Type a step first."); return; }
    var prof = activeProfile();
    prof.steps = prof.steps || [];
    prof.steps.push({ id: uid(), name: name });
    input.value = ""; saveStore(); render();
  });

  document.getElementById("addTimerBtn").addEventListener("click", function(){
    var nameEl = document.getElementById("newTimerName");
    var minsEl = document.getElementById("newTimerMins");
    var name = nameEl.value.trim();
    var mins = parseInt(minsEl.value, 10);
    if (!name){ showMsg("Type a timer name first."); return; }
    if (!(mins > 0)){ showMsg("Enter the minutes."); return; }
    var prof = activeProfile();
    prof.timers = prof.timers || [];
    prof.timers.push({ id: uid(), name: name, minutes: mins, doneText: name.toUpperCase() + " \u2014 TIME'S UP" });
    nameEl.value = ""; minsEl.value = "120";
    saveStore(); render();
  });

  document.getElementById("editBtn").addEventListener("click", function(){
    editMode = !editMode;
    renaming = null; editingTimer = null;
    render();
  });

  document.getElementById("reset").addEventListener("click", function(){
    if (marks[selected]) delete marks[selected];
    if (marks.steps && marks.steps[selected]) delete marks.steps[selected];
    saveMarks(); render();
  });

  function setScale(v){
    scale = Math.min(2, Math.max(0.8, Math.round(v*10)/10));
    document.documentElement.style.setProperty("--scale", scale);
    try { localStorage.setItem("routine:scale", String(scale)); } catch(e){}
  }
  document.getElementById("bigger").addEventListener("click", function(){ setScale(scale + 0.1); });
  document.getElementById("smaller").addEventListener("click", function(){ setScale(scale - 0.1); });

  try {
    var s = parseFloat(localStorage.getItem("routine:scale"));
    if (s >= 0.8 && s <= 2) scale = s;
  } catch(e){}
  document.documentElement.style.setProperty("--scale", scale);

  loadStore();
  loadMarks();
  loadTimers();
  renderDayPicker();
  document.getElementById("today").textContent =
    FULL[todayCode] + " \u2014 " + now.toLocaleDateString(undefined, { month:"long", day:"numeric" });
  render();
  setInterval(tick, 1000);
})();
</script>
</body>
</html>
