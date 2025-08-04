# Aviator-bot


<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Aviator Bot Simulador</title>
<link rel="manifest" href="manifest.json" />
<link rel="stylesheet" href="style.css" />
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
<div id="app">
  <header>
    <h1>Aviator Bot Simulador</h1>
    <button id="toggle-theme">Modo Escuro</button>
  </header>
  <main>
    <section id="chat">
      <div id="messages"></div>
      <input id="input-message" type="text" placeholder="Digite sua mensagem..." autocomplete="off" />
      <button id="send-btn">Enviar</button>
    </section>
    <section id="stats">
      <canvas id="roundChart"></canvas>
      <div id="history"></div>
    </section>
  </main>
</div>

<script src="script.js"></script>
</body>
</html>


body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  margin: 0; padding: 0;
  background: #e5ddd5;
  color: #222;
  display: flex;
  flex-direction: column;
  height: 100vh;
}
#app {
  display: flex;
  flex-direction: column;
[4/8 14:37] ChatGPT: height: 100vh;
  max-width: 480px;
  margin: auto;
  background: white;
  box-shadow: 0 0 10px rgba(0,0,0,0.1);
}
header {
  background: #075e54;
  color: white;
  padding: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
header h1 {
  font-weight: 600;
  font-size: 1.2rem;
}
#toggle-theme {
  background: #25d366;
  border: none;
  border-radius: 4px;
  color: white;
  padding: 0.3rem 0.7rem;
  cursor: pointer;
}
main {
  flex: 1;
  display: flex;
  flex-direction: column;
  padding: 0.5rem;
  overflow: hidden;
}
#chat {
  flex: 1;
  display: flex;
  flex-direction: column;
  border: 1px solid #ddd;
  border-radius: 6px;
  padding: 0.5rem;
  background: #fff;
}
#messages {
  flex: 1;
  overflow-y: auto;
  padding: 0.5rem;
  background: #f0f0f0;
  border-radius: 6px;
  margin-bottom: 0.5rem;
}
.message {
  margin-bottom: 8px;
  padding: 0.5rem 0.7rem;
  border-radius: 20px;
  max-width: 75%;
  word-wrap: break-word;
}
.message.user {
  background: #dcf8c6;
  align-self: flex-end;
}
.message.bot {
  background: #fff;
  border: 1px solid #ccc;
  align-self: flex-start;
}
#input-message {
  padding: 0.5rem;
  border-radius: 20px;
  border: 1px solid #ccc;
  width: calc(100% - 70px);
  margin-right: 0.5rem;
}
#send-btn {
  background: #25d366;
[4/8 14:37] ChatGPT: border: none;
  color: white;
  border-radius: 20px;
  padding: 0.5rem 1rem;
  cursor: pointer;
}
#stats {
  margin-top: 0.5rem;
}
#history {
  max-height: 120px;
  overflow-y: auto;
  background: #fafafa;
  padding: 0.5rem;
  border-radius: 6px;
  font-size: 0.85rem;
  color: #333;
}
body.dark {
  background: #121b22;
  color: #eee;
}
body.dark #app {
  background: #1e2c37;
  box-shadow: none;
}
body.dark header {
  background: #0b3d2e;
}
body.dark #toggle-theme {
  background: #128c7e;
}
body.dark #messages {
  background: #253239;
}
body.dark .message.user {
  background: #056240;
}
body.dark .message.bot {
  background: #17343a;
  border-color: #555;
}

const messagesEl = document.getElementById('messages');
const inputEl = document.getElementById('input-message');
const sendBtn = document.getElementById('send-btn');
const toggleThemeBtn = document.getElementById('toggle-theme');
const historyEl = document.getElementById('history');

let rounds = [];
let isDark = false;
let chart;

function addMessage(text, sender = 'bot') {
  const div = document.createElement('div');
  div.className = `message ${sender}`;
  div.textContent = text;
  messagesEl.appendChild(div);
  messagesEl.scrollTop = messagesEl.scrollHeight;
}

function generateRound() {
[4/8 14:37] ChatGPT: // Simula um multiplicador entre 1.00 e 20.00, mais realista perto do Aviator
  const chance = Math.random();
  let multiplier;
  if (chance < 0.5) multiplier = (Math.random() * 3 + 1).toFixed(2);
  else if (chance < 0.8) multiplier = (Math.random() * 5 + 3).toFixed(2);
  else multiplier = (Math.random() * 15 + 8).toFixed(2);
  return parseFloat(multiplier);


function addRound(multiplier) 
  rounds.push(multiplier);
  if (rounds.length > 50) rounds.shift();
  updateHistory();
  updateChart();


function updateHistory() 
  historyEl.innerHTML = rounds.map((m, i) => `Rodada{i + 1}: x${m}`).join('<br>');
}

function updateChart() {
  if (!chart) {
    const ctx = document.getElementById('roundChart').getContext('2d');
    chart = new Chart(ctx, {
      type: 'line',
      data: {
        labels: rounds.map((_, i) => i + 1),
        datasets: [{
          label: 'Multiplicador',
          data: rounds,
          borderColor: '#25d366',
          backgroundColor: 'rgba(37, 211, 102, 0.2)',
          fill: true,
          tension: 0.3,
        }]
      },
      options: {
        animation: false,
        scales: {
          y: { beginAtZero: true, max: 25 }
        }
      }
    });
  } else {
    chart.data.labels = rounds.map((_, i) => i + 1);
[4/8 14:38] ChatGPT: function playRound() 
  const multiplier = generateRound();
  addRound(multiplier);
  addMessage(`Rodada finalizada! Multiplicador: x{multiplier.toFixed(2)}`);
  notify(`Rodada: x${multiplier.toFixed(2)}`);
  autoStrategy(multiplier);
}

function autoStrategy(multiplier) {
  // Exemplo simples de estratégia:
  // Entra quando o multiplicador anterior foi < 3 e sai em 2x
  if (rounds.length < 2) return;
  const last = rounds[rounds.length - 2];
  if (last < 3) {
    addMessage('Entrada sugerida: Apostar agora!');
    // Espera 5 segundos para saída simulada
    setTimeout(() => {
      addMessage('Saída sugerida: Retirar aos 2x!');
    }, 5000);
  }
}

function notify(text) {
  if (!("Notification" in window)) return;
  if (Notification.permission === "granted") {
    new Notification(text);
  } else if (Notification.permission !== "denied") {
    Notification.requestPermission().then(permission => {
      if (permission === "granted") new Notification(text);
    });
  }
}

sendBtn.onclick = () => {
  const text = inputEl.value.trim();
  if (!text) return;
  addMessage(text, 'user');
  inputEl.value = '';
  setTimeout(() => {
    handleUserMessage(text);
  }, 500);
};

function handleUserMessage(text) {
  const lower = text.toLowerCase();
  if (lower.includes('rodada')) {
[4/8 14:38] ChatGPT: else if (lower.includes('histórico') || lower.includes('history')) 
    addMessage(`Histórico:{rounds.map((m, i) => `Rod i+1: x{m}`).join(', ')}`);
  } else if (lower.includes('ajuda')) {
    addMessage('Digite "rodada" para simular uma rodada, "histórico" para ver o histórico.');
  } else {
    addMessage('Não entendi. Digite "ajuda" para comandos.');
  }
}

toggleThemeBtn.onclick = () => {
  isDark = !isDark;
  if (isDark) document.body.classList.add('dark');
  else document.body.classList.remove('dark');
};

// Inicialização
updateHistory();
updateChart();

{
  "name": "Aviator Bot Simulador",
  "short_name": "AviatorBot",
  "start_url": ".",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#25d366",
  "icons": [
    {
      "src": "icon.png",
      "sizes": "192x192",
      "type": "image/png"
    }
  ]
}

const cacheName = 'aviator-bot-cache-v1';
const assetsToCache = [
  '.',
  'index.html',
  'style.css',
  'script.js',
  'manifest.json',
  'https://cdn.jsdelivr.net/npm/chart.js',
  'icon.png'
];

self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(cacheName)
    .then(cache => cache.addAll(assetsToCache))
  );
});

self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request)
    .then(res => res || fetch(event.request))
  );
});
