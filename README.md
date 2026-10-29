<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="theme-color" content="#fff7e8">
<title>A & B Adventure!</title>
<style>
:root{--ink:#25324a;--pink:#ff7894;--yellow:#ffd65a;--blue:#9fe3f5;--green:#b9edc5;--purple:#cbb7ff;--paper:#fffdf8}
*{box-sizing:border-box}
body{margin:0;background:linear-gradient(160deg,#fff7e8,#f1f8ff);color:var(--ink);font-family:ui-rounded,"Trebuchet MS",system-ui,sans-serif;min-height:100vh}
button{font:inherit;touch-action:manipulation;cursor:pointer}
.app{max-width:850px;margin:auto;padding:clamp(12px,3vw,28px);padding-bottom:32px}
header{text-align:center;padding:10px 0 18px}
h1{font-size:clamp(2rem,7vw,3.4rem);line-height:1.05;margin:6px 0;color:#5a49a8;letter-spacing:-1px}
.subtitle{font-size:clamp(1rem,3vw,1.2rem);margin:8px 0}
.stars{display:inline-flex;align-items:center;gap:7px;background:#fff;border:2px solid #f3e4b4;border-radius:999px;padding:8px 14px;font-weight:800}
.tabs{display:grid;grid-template-columns:repeat(4,1fr);gap:8px;margin:14px 0 16px}
.tab{border:0;border-radius:16px;padding:12px 5px;background:#fff;box-shadow:0 3px 0 #e5e1d8;color:var(--ink);font-weight:800}
.tab[aria-selected="true"]{background:var(--purple);box-shadow:0 3px 0 #a38be9}
.panel{background:rgba(255,255,255,.92);border:3px solid #fff;border-radius:28px;padding:clamp(16px,4vw,28px);box-shadow:0 12px 30px #45506b12;min-height:360px}
h2{font-size:clamp(1.4rem,4vw,2rem);margin:0 0 8px}
p{line-height:1.5}
.letter-pair{display:flex;justify-content:center;gap:clamp(20px,8vw,70px);margin:22px auto}
.letter-card{width:min(34vw,190px);min-height:170px;border-radius:26px;display:flex;flex-direction:column;align-items:center;justify-content:center;border:3px solid #fff;box-shadow:0 5px 0 #e4e4e4}
.letter-card:first-child{background:#ffe1e8}.letter-card:last-child{background:#dff5ff}
.big-letter{font-size:clamp(4.5rem,20vw,7rem);font-weight:1000;line-height:1}
.small-label{font-weight:800;margin-top:8px}
.action-row{display:flex;justify-content:center;flex-wrap:wrap;gap:10px;margin-top:18px}
.primary,.secondary,.choice{border:0;border-radius:16px;padding:13px 18px;font-weight:900;min-height:48px}
.primary{background:var(--yellow);box-shadow:0 4px 0 #e6b93e}
.secondary{background:#edf0ff;box-shadow:0 4px 0 #d2d8f5}
.feedback{font-size:1.12rem;font-weight:800;text-align:center;min-height:32px;margin:16px 0 0}
.word-grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:12px;margin-top:16px}
.word-card{background:#fff;border:3px solid #eee;border-radius:20px;padding:16px 8px;text-align:center;min-height:122px}
.word-card:active{transform:scale(.98)}
.emoji{font-size:clamp(2.5rem,10vw,4rem);display:block;margin-bottom:5px}
.word{font-size:1.1rem;font-weight:900}
.letter-tag{display:inline-block;border-radius:99px;padding:3px 9px;margin-top:7px;font-size:.85rem;font-weight:900;background:#f0eaff}
.question{font-size:clamp(1.6rem,5vw,2.4rem);font-weight:1000;text-align:center;margin:18px 0}
.choices{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:12px}
.choice{background:#f2f7ff;border:3px solid #dbe9ff;font-size:1.2rem}
.choice.correct{background:#c9f4d3;border-color:#7edb94}
.choice.wrong{background:#ffe0e5;border-color:#ff9bad}
.write-box{font-size:3.5rem;text-align:center;letter-spacing:8px;background:#fff8db;border:3px dashed #f1cf59;border-radius:20px;padding:16px;margin:18px 0}
input{display:block;width:100%;font:inherit;font-size:1.3rem;text-align:center;border:3px solid #dce4f6;border-radius:16px;padding:14px;color:var(--ink);background:#fff}
input:focus{outline:3px solid #cbb7ff}
.note{font-size:.94rem;color:#5e6980;text-align:center}
footer{text-align:center;font-size:.85rem;color:#66718a;padding:18px 0 0}
@media(min-width:620px){.word-grid{grid-template-columns:repeat(4,minmax(0,1fr))}.panel{min-height:400px}}
@media(max-width:380px){.tabs{gap:5px}.tab{font-size:.85rem;padding:11px 2px}.panel{padding:14px}.word-grid{gap:8px}}
</style>
</head>
<body>
<div class="app">
<header>
  <div class="stars" aria-live="polite">⭐ Stars: <span id="starCount">0</span></div>
  <h1>A &amp; B Adventure!</h1>
  <p class="subtitle">Let’s learn letters, listen, match and write! 🌈</p>
</header>
<nav class="tabs" aria-label="Game activities">
  <button class="tab" data-tab="letters" aria-selected="true">🔤 Letters</button>
  <button class="tab" data-tab="listen" aria-selected="false">👂 Listen</button>
  <button class="tab" data-tab="match" aria-selected="false">🧩 Match</button>
  <button class="tab" data-tab="write" aria-selected="false">✏️ Write</button>
</nav>
<main class="panel" id="panel" aria-live="polite"></main>
<footer>Made for little learners · Tap the speaker to hear each word 🔊</footer>
</div>
<script>
const words=[
 {word:"apple",emoji:"🍎",letter:"A"},{word:"axe",emoji:"🪓",letter:"A"},
 {word:"ant",emoji:"🐜",letter:"A"},{word:"alligator",emoji:"🐊",letter:"A"},
 {word:"bear",emoji:"🐻",letter:"B"},{word:"bird",emoji:"🐦",letter:"B"},
 {word:"bed",emoji:"🛏️",letter:"B"},{word:"banana",emoji:"🍌",letter:"B"}
];
let stars=0, active="letters", listenIndex=0, matchItem=null, writeIndex=0, matchDone=false;
const panel=document.getElementById("panel");
function say(text){ if(!("speechSynthesis" in window)){feedback("Audio is not available on this device.");return;} speechSynthesis.cancel(); const u=new SpeechSynthesisUtterance(text);u.lang="en-US";u.rate=.82;speechSynthesis.speak(u);}
function reward(){stars++;document.getElementById("starCount").textContent=stars;}
function feedback(text){const el=document.getElementById("feedback");if(el)el.textContent=text;}
function setTab(tab){active=tab;document.querySelectorAll(".tab").forEach(b=>b.setAttribute("aria-selected",String(b.dataset.tab===tab)));render();}
document.querySelectorAll(".tab").forEach(b=>b.addEventListener("click",()=>setTab(b.dataset.tab)));
function render(){
 if(active==="letters") renderLetters();
 if(active==="listen") renderListen();
 if(active==="match") renderMatch();
 if(active==="write") renderWrite();
}
function renderLetters(){
 panel.innerHTML=`<h2>Meet A and B!</h2><p>Tap a letter to hear its name and sound.</p>
 <div class="letter-pair">
 <button class="letter-card" id="letterA" aria-label="Hear letter A"><span class="big-letter">Aa</span><span class="small-label">A says /æ/</span></button>
 <button class="letter-card" id="letterB" aria-label="Hear letter B"><span class="big-letter">Bb</span><span class="small-label">B says /b/</span></button>
 </div><p class="note">A is for apple. B is for bear!</p>
 <div class="action-row"><button class="primary" id="letterNext">Let’s explore! ✨</button></div><div class="feedback" id="feedback"></div>`;
 document.getElementById("letterA").onclick=()=>{say("A. A says ah. A is for apple.");reward();feedback("Amazing! A is for apple! 🍎")};
 document.getElementById("letterB").onclick=()=>{say("B. B says buh. B is for bear.");reward();feedback("Brilliant! B is for bear! 🐻")};
 document.getElementById("letterNext").onclick=()=>setTab("listen");
}
function renderListen(){
 const item=words[listenIndex%words.length];
 panel.innerHTML=`<h2>Listen and say it!</h2><p>Look at the picture, then tap the speaker.</p>
 <button class="word-card" style="display:block;width:min(100%,280px);margin:18px auto;background:#fff3d2" id="speakWord"><span class="emoji">${item.emoji}</span><span class="word">🔊 Hear the word</span></button>
 <div class="question">What do you hear?</div>
 <div class="choices"><button class="choice" data-answer="A">A</button><button class="choice" data-answer="B">B</button></div>
 <div class="feedback" id="feedback"></div><div class="action-row"><button class="secondary" id="nextListen">Next picture ➜</button></div>`;
 document.getElementById("speakWord").onclick=()=>say(item.word);
 document.querySelectorAll("[data-answer]").forEach(b=>b.onclick=()=>{
   if(b.dataset.answer===item.letter){b.classList.add("correct");feedback(`Yes! ${item.word} starts with ${item.letter}! 🎉`);say(item.word);reward();}
   else{b.classList.add("wrong");feedback("Good try! Listen again and try once more. 💛");}
 });
 document.getElementById("nextListen").onclick=()=>{listenIndex=(listenIndex+1)%words.length;renderListen()};
}
function renderMatch(){
 const pool=words.slice(0,4); // friendly first round: apple, axe, ant, alligator
 if(!matchItem){matchItem=pool[Math.floor(Math.random()*pool.length)];}
 panel.innerHTML=`<h2>Match the first letter!</h2><p>Which letter does this word begin with?</p>
 <div class="question">${matchItem.emoji} ${matchItem.word}</div>
 <div class="choices"><button class="choice" data-match="A">A</button><button class="choice" data-match="B">B</button></div>
 <div class="feedback" id="feedback"></div><div class="action-row"><button class="secondary" id="newMatch">New word ↻</button></div>`;
 document.querySelectorAll("[data-match]").forEach(b=>b.onclick=()=>{
 if(b.dataset.match===matchItem.letter){b.classList.add("correct");feedback(`Great matching! ${matchItem.word} starts with ${matchItem.letter}. ⭐`);say(matchItem.word);reward();}
 else{b.classList.add("wrong");feedback("Almost! Tap the word to hear it, then try again. 😊");}
 });
 document.getElementById("newMatch").onclick=()=>{matchItem=words[Math.floor(Math.random()*words.length)];renderMatch()};
}
function renderWrite(){
 const item=writeIndex%2===0?{letter:"A",word:"apple",emoji:"🍎"}:{letter:"B",word:"bear",emoji:"🐻"};
 panel.innerHTML=`<h2>Let’s practise writing!</h2><p>Look at the big letter, then type it in the box.</p>
 <div class="write-box">${item.letter} ${item.letter.toLowerCase()}</div>
 <p class="question">${item.emoji} ${item.word}</p>
 <form id="writeForm"><input id="letterInput" aria-label="Type the letter" maxlength="1" autocomplete="off" autocapitalize="characters" placeholder="Type A or B"><div class="action-row"><button class="primary" type="submit">Check my letter ✓</button></div></form>
 <div class="feedback" id="feedback"></div><div class="action-row"><button class="secondary" id="nextWrite">Next letter ➜</button></div>`;
 document.getElementById("writeForm").onsubmit=e=>{e.preventDefault();const value=document.getElementById("letterInput").value.trim().toUpperCase();if(value===item.letter){feedback("Wonderful writing practice! ⭐");say(item.letter);reward();}else feedback("Try again! Look at the big letter above. 💛")};
 document.getElementById("nextWrite").onclick=()=>{writeIndex++;renderWrite()};
}
render();
</script>
</body>
</html>
