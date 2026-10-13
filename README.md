# islamic-tools
Aasif Islamic Tools — Quran, Hadith and Islamic tools
<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#14532d">
<title>Aasif Islamic Tools</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#f5f7ef;color:#183528;
font-family:Arial,sans-serif}
header{background:#14532d;color:white;text-align:center;
padding:24px 12px}
header h1{margin:0 0 8px}
nav{display:flex;justify-content:center;gap:16px;
flex-wrap:wrap;margin-top:16px}
a{color:#17603a}
nav a{color:white;text-decoration:none}
main{max-width:600px;margin:22px auto;padding:0 14px}
.card{background:white;border-radius:16px;padding:22px;
margin-bottom:18px;box-shadow:0 3px 14px #0000000b}
h2{margin-top:0}
#count{font-size:64px;text-align:center;
font-weight:bold;margin:22px 0;color:#14532d}
button{border:0;border-radius:10px;padding:13px 17px;
font-size:16px;cursor:pointer}
.primary{background:#166534;color:white;width:100%;
min-height:58px;font-size:20px}
.row{display:flex;gap:10px;margin-top:12px}
.row button{flex:1;background:#e4eee4;color:#14532d}
p{line-height:1.65}
footer{text-align:center;padding:20px;color:#536458}
</style>
</head>
<body>
<header>
  <h1>🌙 Aasif Islamic Tools</h1>
  <p>Islamic maloomat aur useful tools</p>
  <nav>
    <a href="#tasbeeh">Tasbeeh</a>
    <a href="#quran">Quran</a>
    <a href="#hadith">Hadith</a>
    <a href="#history">Islamic History</a>
  </nav>
</header>

<main>
  <section class="card" id="tasbeeh">
    <h2>📿 Digital Tasbeeh Counter</h2>
    <p>Apne zikr ki ginti yahan karein.</p>
    <div id="count" aria-live="polite">0</div>
    <button class="primary" id="plus">+1 Zikr</button>
    <div class="row">
      <button id="minus">−1</button>
      <button id="reset">Reset</button>
    </div>
    <p id="message" aria-live="polite"></p>
    <p><small>Note: Page refresh karne par count reset ho jayega.</small></p>
  </section>

  <section class="card" id="quran">
    <h2>📖 Quran</h2>
    <p>Quran padhne aur surah dhoondhne ke liye:</p>
    <a href="https://quran.com/" target="_blank"
       rel="noopener noreferrer">Quran.com kholen ↗</a>
  </section>

  <section class="card" id="hadith">
    <h2>📚 Hadith</h2>
    <p>Hadith padhte waqt kitab, reference aur authenticity
    ki details check karein.</p>
    <a href="https://sunnah.com/" target="_blank"
       rel="noopener noreferrer">Sunnah.com kholen ↗</a>
  </section>

  <section class="card" id="history">
    <h2>🕌 Islamic History</h2>
    <p>Yahan Islamic history ke verified articles,
    tareekhi waqiat aur unke references publish kiye jayenge.</p>
    <p>Har article mein source aur tareekhi ikhtilaf
    ko zaroorat ke mutabiq wazeh karein.</p>
  </section>
</main>

<footer>
  © <span id="year"></span> Aasif Islamic Tools
</footer>

<script>
let count = 0;
const display = document.getElementById("count");
const message = document.getElementById("message");

function updateCount() {
  display.textContent = count;
  message.textContent = count > 0
    ? "Aapka zikr count: " + count
    : "Zikr shuru karne ke liye +1 dabayein.";
}

document.getElementById("plus").onclick = () => {
  count++;
  updateCount();
};

document.getElementById("minus").onclick = () => {
  if (count > 0) count--;
  updateCount();
};

document.getElementById("reset").onclick = () => {
  count = 0;
  updateCount();
};

document.getElementById("year").textContent =
  new Date().getFullYear();

updateCount();
</script>
</body>
</html>
