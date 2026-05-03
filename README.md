<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Left and Right Game</title>

<style>
body{
  margin:0;
  font-family:"Comic Sans MS", Arial;
  background:linear-gradient(135deg,#b388eb,#8ec5fc);
  display:flex;
  justify-content:center;
  align-items:center;
  height:100vh;
}
.game{
  background:white;
  padding:20px;
  border-radius:20px;
  width:90%;
  max-width:900px;
  text-align:center;
}
h1{font-size:40px}
#question{font-size:24px;margin:15px}

.objects{
  display:flex;
  justify-content:space-around;
  margin:20px;
}

.objects img{
  width:120px;
}

.options{
  display:grid;
  grid-template-columns:repeat(2,1fr);
  gap:15px;
}

button{
  padding:10px;
  border-radius:12px;
  border:none;
  background:#4dabf7;
}

button img{
  width:80px;
}

.correct{background:#51cf66}
.wrong{background:#ff6b6b}

.mark{
  position:fixed;
  top:50%;left:50%;
  transform:translate(-50%,-50%);
  font-size:120px;
  display:none;
}
.show{display:block}
</style>
</head>

<body>

<div class="game">
<h1>Left & Right</h1>
<div id="question"></div>

<div class="objects" id="objects"></div>
<div class="options" id="options"></div>

<div id="score">Score: 0</div>
</div>

<div id="tick" class="mark">👏</div>
<div id="cross" class="mark">❌</div>

<script>

const questions = [
{
dir:"RIGHT",
name:"parrot",
objs:[
"https://cdn-icons-png.flaticon.com/512/616/616490.png",
"https://cdn-icons-png.flaticon.com/512/3069/3069172.png",
"https://cdn-icons-png.flaticon.com/512/616/616554.png"
],
ans:"https://cdn-icons-png.flaticon.com/512/3069/3069172.png"
},
{
dir:"LEFT",
name:"ladybug",
objs:[
"https://cdn-icons-png.flaticon.com/512/616/616408.png",
"https://cdn-icons-png.flaticon.com/512/616/616430.png",
"https://cdn-icons-png.flaticon.com/512/616/616408.png"
],
ans:"https://cdn-icons-png.flaticon.com/512/616/616408.png"
},
{
dir:"RIGHT",
name:"apple",
objs:[
"https://cdn-icons-png.flaticon.com/512/415/415682.png",
"https://cdn-icons-png.flaticon.com/512/590/590685.png",
"https://cdn-icons-png.flaticon.com/512/590/590682.png"
],
ans:"https://cdn-icons-png.flaticon.com/512/590/590685.png"
},
{
dir:"LEFT",
name:"car",
objs:[
"https://cdn-icons-png.flaticon.com/512/743/743131.png",
"https://cdn-icons-png.flaticon.com/512/743/743007.png",
"https://cdn-icons-png.flaticon.com/512/743/743922.png"
],
ans:"https://cdn-icons-png.flaticon.com/512/743/743131.png"
}
];

let current=0,score=0;

function speak(text){
  const msg=new SpeechSynthesisUtterance(text);
  msg.rate=0.8;
  speechSynthesis.cancel();
  speechSynthesis.speak(msg);
}

function loadQuestion(){
  const q=questions[current];

  const text="Click the object to the "+q.dir.toLowerCase()+" of the "+q.name;
  document.getElementById("question").innerText=text;
  speak(text);

  const objDiv=document.getElementById("objects");
  objDiv.innerHTML="";
  q.objs.forEach(o=>{
    const img=document.createElement("img");
    img.src=o;
    objDiv.appendChild(img);
  });

  const opts=document.getElementById("options");
  opts.innerHTML="";
  shuffle([...q.objs]).forEach(o=>{
    const btn=document.createElement("button");
    const img=document.createElement("img");
    img.src=o;
    btn.appendChild(img);
    btn.onclick=()=>check(o,btn);
    opts.appendChild(btn);
  });
}

function check(ans,btn){
  const correct=questions[current].ans;

  if(ans===correct){
    btn.classList.add("correct");
    score++;
    show("tick");
    speak("Good job");
    next();
  }else{
    btn.classList.add("wrong");
    show("cross");
    speak("Try again");
  }

  document.getElementById("score").innerText="Score: "+score;
}

function next(){
  setTimeout(()=>{
    current++;
    if(current>=questions.length){
      alert("Great Job! Score: "+score);
      location.reload();
    }else loadQuestion();
  },1200);
}

function show(id){
  const el=document.getElementById(id);
  el.classList.add("show");
  setTimeout(()=>el.classList.remove("show"),800);
}

function shuffle(a){return a.sort(()=>Math.random()-0.5)}

loadQuestion();

</script>

</body>
</html>
