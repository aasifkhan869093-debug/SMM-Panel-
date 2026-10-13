
<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Aasif Ludo</title>
<style>
*{box-sizing:border-box}
body{margin:0;padding:16px;background:#101827;color:white;
font-family:Arial,sans-serif;text-align:center}
h1{color:#86efac}
.info{background:#1e293b;padding:14px;border-radius:14px;margin:12px auto;max-width:460px}
#board{display:grid;grid-template-columns:repeat(15,1fr);
width:min(94vw,460px);aspect-ratio:1;margin:18px auto;
border:3px solid white;background:white;gap:1px}
.cell{position:relative;background:#f8fafc;min-width:0}
.red{background:#fecaca}
.green{background:#bbf7d0}
.yellow{background:#fef08a}
.blue{background:#bfdbfe}
.safe{background:#e2e8f0}
.home{background:conic-gradient(#16a34a 0 25%,#eab308 0 50%,#2563eb 0 75%,#dc2626 0)}
.token{position:absolute;inset:8%;border-radius:50%;
border:2px solid white;display:flex;align-items:center;
justify-content:center;font-size:clamp(8px,2.5vw,14px);
font-weight:bold;color:white;cursor:pointer;box-shadow:0 1px 4px #333}
.token:focus{outline:3px solid black}
button,select{padding:12px;border:0;border-radius:10px;font-size:16px}
#roll{background:#22c55e;color:#052e16;font-weight:bold;width:100%;max-width:300px}
#dice{font-size:48px;margin:8px}
.note{font-size:13px;color:#cbd5e1;line-height:1.6}
</style>
</head>
<body>
<h1>🎲 Aasif Ludo</h1>
<div class="info">
  <div id="turn">लाल खिलाड़ी की बारी</div>
  <div id="dice">🎲</div>
  <button id="roll">पासा फेंको</button>
  <p id="msg">पासा फेंककर खेल शुरू करो।</p>
  <label for="players">खिलाड़ी:</label>
  <select id="players">
    <option value="2">2 खिलाड़ी</option>
    <option value="3">3 खिलाड़ी</option>
    <option value="4" selected>4 खिलाड़ी</option>
  </select>
  <button id="restart">नई गेम</button>
</div>
<div id="board" aria-label="Ludo board"></div>
<p class="note">यह शुरुआती स्थानीय गेम है। सभी खिलाड़ी एक ही डिवाइस पर खेलते हैं।</p>

<script>
const colors=[
 {name:"लाल",css:"#dc2626",base:0},
 {name:"हरा",css:"#16a34a",base:39},
 {name:"पीला",css:"#ca8a04",base:26},
 {name:"नीला",css:"#2563eb",base:13}
];
const path=[
 [6,1],[6,2],[6,3],[6,4],[6,5],[5,6],[4,6],[3,6],[2,6],[1,6],[0,6],
 [0,7],[0,8],[1,8],[2,8],[3,8],[4,8],[5,8],[6,9],[6,10],[6,11],
 [6,12],[6,13],[6,14],[7,14],[8,14],[8,13],[8,12],[8,11],[8,10],
 [8,9],[9,8],[10,8],[11,8],[12,8],[13,8],[14,8],[14,7],[14,6],
 [13,6],[12,6],[11,6],[10,6],[9,6],[8,5],[8,4],[8,3],[8,2],[8,1],
 [8,0],[7,0],[6,0]
];
const board=document.getElementById("board");
const msg=document.getElementById("msg");
const dice=document.getElementById("dice");
const turnText=document.getElementById("turn");
let players=4,turn=0,lastRoll=0,needMove=false,finished=false;
let pieces=[];

function startGame(){
 players=Number(document.getElementById("players").value);
 turn=0;lastRoll=0;needMove=false;finished=false;
 pieces=colors.map(()=>[-1,-1,-1,-1]);
 render();
 dice.textContent="🎲";
 msg.textContent="पासा फेंककर खेल शुरू करो।";
 turnText.textContent=colors[turn].name+" खिलाड़ी की बारी";
 document.getElementById("roll").disabled=false;
}
function render(){
 board.innerHTML="";
 for(let r=0;r<15;r++)for(let c=0;c<15;c++){
  let cell=document.createElement("div");cell.className="cell";
  if(r<6&&c<6)cell.classList.add("red");
  if(r<6&&c>8)cell.classList.add("green");
  if(r>8&&c<6)cell.classList.add("blue");
  if(r>8&&c>8)cell.classList.add("yellow");
  if((r===7&&c===7))cell.classList.add("home");
  let idx=path.findIndex(p=>p[0]===r&&p[1]===c);
  if(idx>=0)cell.classList.add("safe");
  cell.id="c"+r+"-"+c;
  board.appendChild(cell);
 }
 for(let p=0;p<players;p++){
  for(let t=0;t<4;t++){
   let pos=pieces[p][t],r,c;
   if(pos===-1){
    const bases=[
      [[1,1],[1,4],[4,1],[4,4]],
      [[1,10],[1,13],[4,10],[4,13]],
      [[10,10],[10,13],[13,10],[13,13]],
      [[10,1],[10,4],[13,1],[13,4]]
    ];
    [r,c]=bases[p][t];
   }else{
    let ix=(colors[p].base+pos)%path.length;
    [r,c]=path[ix];
   }
   let cell=document.getElementById("c"+r+"-"+c);
   let token=document.createElement("button");
   token.className="token";
   token.style.background=colors[p].css;
   token.textContent=t+1;
   token.title=colors[p].name+" गोटी "+(t+1);
   token.onclick=()=>movePiece(p,t);
   if(p!==turn||!needMove)token.style.opacity=".8";
   cell.appendChild(token);
  }
 }
}
function roll(){
 if(finished||needMove)return;
 lastRoll=Math.floor(Math.random()*6)+1;
 dice.textContent=["","⚀","⚁","⚂","⚃","⚄","⚅"][lastRoll];
 if(lastRoll!==6&&!pieces[turn].some(x=>x>=0)){
  msg.textContent="6 नहीं आया। अगली बारी!";
  nextTurn();return;
 }
 needMove=true;
 msg.textContent=lastRoll===6
  ?"6 आया! अपनी गोटी चुनो।"
  :"अब अपनी चलने वाली गोटी चुनो।";
 render();
}
function movePiece(p,t){
 if(finished||p!==turn||!needMove)return;
 let pos=pieces[p][t];
 if(pos===-1){
  if(lastRoll!==6){msg.textContent="गोटी निकालने के लिए 6 चाहिए।";return;}
  pieces[p][t]=0;
 }else{
  let next=pos+lastRoll;
  if(next>=path.length){msg.textContent="इस गोटी की चाल यहाँ पूरी नहीं हो सकती।";return;}
  pieces[p][t]=next;
  let abs=(colors[p].base+next)%path.length;
  for(let op=0;op<players;op++){
   if(op===p)continue;
   for(let ot=0;ot<4;ot++){
    if(pieces[op][ot]>=0&&
       (colors[op].base+pieces[op][ot])%path.length===abs){
      pieces[op][ot]=-1;
    }
   }
  }
 }
 needMove=false;
 if(pieces[p].every(x=>x===path.length-1)){
  finished=true;
  msg.textContent=colors[p].name+" खिलाड़ी जीत गया!";
  document.getElementById("roll").disabled=true;
  render();return;
 }
 if(lastRoll!==6)nextTurn();
 else msg.textContent="6 आया! आपको एक और मौका मिलेगा।";
 render();
}
function nextTurn(){
 needMove=false;
 turn=(turn+1)%players;
 turnText.textContent=colors[turn].name+" खिलाड़ी की बारी";
}
document.getElementById("roll").onclick=roll;
document.getElementById("restart").onclick=startGame;
document.getElementById("players").onchange=startGame;
startGame();
</script>
</body>
</html>
