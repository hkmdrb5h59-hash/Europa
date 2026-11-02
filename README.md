# 
<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<meta name="theme-color" content="#635bdb">
<title>Europa-Entdecker</title>
<style>
*{box-sizing:border-box}
body{margin:0;padding:20px;font-family:Arial,sans-serif;background:#f0efff;color:#26324b}
main{max-width:650px;margin:auto}
header{text-align:center;padding:20px}
h1{color:#554bc5;font-size:32px}
.card{background:white;padding:22px;border-radius:24px;box-shadow:0 5px 20px #0001}
button{width:100%;padding:16px;margin:7px 0;border:0;border-radius:14px;background:#635bdb;color:white;font-size:17px;font-weight:bold;cursor:pointer}
button:disabled{opacity:1}
.answer{background:#f0efff;color:#30334d;border:2px solid transparent}
.correct{background:#d8ffed!important;border-color:#00a879!important;color:#087653!important}
.wrong{background:#ffe4e4!important;border-color:#ed7777!important;color:#a22!important}
.small{font-size:14px;color:#737b91;text-align:center}
.hidden{display:none}
.progress{background:#eee;border-radius:20px;height:10px;margin:20px 0}
.bar{background:#00b894;height:10px;border-radius:20px;width:0}
h2{text-align:center;font-size:26px}
#feedback{text-align:center;padding:15px;font-weight:bold}
</style>
</head>
<body>
<main>
<header>
<div style="font-size:55px">🌍✨</div>
<h1>Europa-Entdecker</h1>
<p>Das Hauptstadt-Quiz für Klasse 4</p>
</header>

<section id="home" class="card">
<h2>Willkommen! 👋</h2>
<p class="small">Entdecke die 27 Länder der Europäischen Union.</p>
<button onclick="start(10)">🎯 Quiz starten – 10 Fragen</button>
<button onclick="start(27)">🧠 Alle 27 Länder üben</button>
<button onclick="start(5)">⭐ Kleine Runde – 5 Fragen</button>
<button onclick="learn()">📚 Lernkarten ansehen</button>
</section>

<section id="quiz" class="card hidden">
<button onclick="menu()" style="background:#eee;color:#333">← Menü</button>
<p id="counter" class="small"></p>
<div class="progress"><div id="bar" class="bar"></div></div>
<h2 id="question"></h2>
<div id="answers"></div>
<div id="feedback"></div>
<button id="next" class="hidden" onclick="next()">Weiter ➜</button>
</section>

<section id="result" class="card hidden">
<h2>🏆 Geschafft!</h2>
<h1 id="points" style="text-align:center"></h1>
<p id="resultText" class="small"></p>
<button onclick="start(10)">🔁 Nochmal spielen</button>
<button onclick="menu()" style="background:#eee;color:#333">🏠 Menü</button>
</section>

<section id="learn" class="card hidden">
<button onclick="menu()" style="background:#eee;color:#333">← Menü</button>
<h2>📚 Alle EU-Hauptstädte</h2>
<div id="list"></div>
</section>
</main>

<script>
const data=[
["Belgien","Brüssel"],["Bulgarien","Sofia"],
["Dänemark","Kopenhagen"],["Deutschland","Berlin"],
["Estland","Tallinn"],["Finnland","Helsinki"],
["Frankreich","Paris"],["Griechenland","Athen"],
["Irland","Dublin"],["Italien","Rom"],
["Kroatien","Zagreb"],["Lettland","Riga"],
["Litauen","Vilnius"],["Luxemburg","Luxemburg"],
["Malta","Valletta"],["Niederlande","Amsterdam"],
["Österreich","Wien"],["Polen","Warschau"],
["Portugal","Lissabon"],["Rumänien","Bukarest"],
["Schweden","Stockholm"],["Slowakei","Bratislava"],
["Slowenien","Ljubljana"],["Spanien","Madrid"],
["Tschechien","Prag"],["Ungarn","Budapest"],
["Zypern","Nikosia"]
];

let questions=[],index=0,score=0,answered=false;

function shuffle(a){
 return [...a].sort(()=>Math.random()-.5);
}

function show(id){
 ["home","quiz","result","learn"].forEach(x=>
 document.getElementById(x).classList.toggle("hidden",x!==id));
}

function menu(){show("home")}

function start(count){
 questions=shuffle(data).slice(0,count);
 index=0;score=0;
 show("quiz");
 render();
}

function render(){
 answered=false;
 const [country,capital]=questions[index];
 document.getElementById("counter").textContent=
 "Frage "+(index+1)+" von "+questions.length;
 document.getElementById("bar").style.width=
 (index/questions.length*100)+"%";
 document.getElementById("question").textContent=
 "Wie heißt die Hauptstadt von "+country+"?";
 document.getElementById("feedback").textContent="";
 document.getElementById("next").classList.add("hidden");

 let choices=shuffle([
 capital,
 ...shuffle(data.filter(x=>x[1]!==capital).map(x=>x[1])).slice(0,3)
 ]);

 const box=document.getElementById("answers");
 box.innerHTML="";

 choices.forEach((answer,i)=>{
 let b=document.createElement("button");
 b.className="answer";
 b.textContent=String.fromCharCode(65+i)+" · "+answer;
 b.onclick=()=>check(answer,b);
 box.appendChild(b);
 });
}

function check(answer,button){
 if(answered)return;
 answered=true;
 const correct=questions[index][1];
 const right=answer===correct;
 if(right)score++;

 document.querySelectorAll(".answer").forEach(b=>{
 b.disabled=true;
 if(b.textContent.slice(4)===correct)b.classList.add("correct");
 });

 if(!right)button.classList.add("wrong");

 document.getElementById("feedback").textContent=
 right?"🎉 Richtig! Super gemacht!":
 "😊 Fast! Richtig ist: "+correct;

 document.getElementById("next").textContent=
 index===questions.length-1?"Ergebnis ansehen 🏆":"Weiter ➜";
 document.getElementById("next").classList.remove("hidden");
}

function next(){
 index++;
 if(index>=questions.length){
 show("result");
 document.getElementById("points").textContent=
 score+" von "+questions.length+" Punkten";
 document.getElementById("resultText").textContent=
 score===questions.length?"Perfekt! Du bist ein Europa-Profi!":
 "Weiter üben! Du wirst immer besser.";
 }else render();
}

function learn(){
 show("learn");
 document.getElementById("list").innerHTML=data.map(x=>
 "<p>🌍 "+x[0]+" – <b>"+x[1]+"</b></p>"
 ).join("");
}
</script>
</body>
</html>

