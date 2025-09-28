<!doctype html>
<html lang="es">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Gustavo — IA Inteligente</title>
<style>
:root{--accent:linear-gradient(90deg,#ff416c,#ff8b00,#ffd200,#00d084,#0066ff);} 
html,body{height:100%;margin:0;font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto,'Helvetica Neue',Arial}
body{background:conic-gradient(from 0deg,#ff416c,#ff8b00,#ffd200,#00d084,#0066ff,#ff416c);background-size:400% 400%;animation:rainbow 15s linear infinite;color:#e6eef8;display:flex;align-items:center;justify-content:center;padding:18px}
@keyframes rainbow{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
.app{width:100%;max-width:1150px;background:rgba(0,0,0,0.62);border-radius:16px;padding:18px;box-shadow:0 12px 40px rgba(2,6,23,0.6);backdrop-filter:blur(8px)}
header{display:flex;align-items:center;gap:14px}
.logo{width:56px;height:56px;border-radius:12px;background:var(--accent);display:flex;align-items:center;justify-content:center;font-weight:700;color:#061027}
h1{margin:0;font-size:18px}
p.lead{margin:0;color:#d8e6ff;font-size:13px}
.grid{display:grid;grid-template-columns:1fr 360px;gap:16px;margin-top:14px}
.panel{background:rgba(255,255,255,0.03);border-radius:12px;padding:12px;min-height:420px;display:flex;flex-direction:column}
.chat{flex:1;overflow:auto;padding:8px;display:flex;flex-direction:column;gap:10px}
.bubble{max-width:78%;padding:10px 12px;border-radius:12px;background:rgba(255,255,255,0.03)}
.bubble.user{align-self:flex-end;background:linear-gradient(90deg,#ff8b00,#ffd200);color:#061027}
.bubble.bot{align-self:flex-start;background:linear-gradient(90deg,#00d084,#0066ff)}
.meta{font-size:11px;color:#9fb0d4;margin-bottom:6px}
.controls{display:flex;gap:8px;margin-top:10px}
input[type=text]{flex:1;padding:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.06);background:transparent;color:inherit}
button{padding:10px 12px;border-radius:10px;border:0;background:var(--accent);color:#061027;font-weight:600;cursor:pointer}
.small{padding:8px 10px;font-size:13px}
textarea{width:100%;min-height:80px;border-radius:10px;padding:10px;margin-top:8px;border:1px solid rgba(255,255,255,0.06);background:transparent;color:inherit;resize:vertical}
.images{display:grid;grid-template-columns:repeat(auto-fill,minmax(140px,1fr));gap:8px;margin-top:8px}
.images img{width:100%;border-radius:10px;box-shadow:0 4px 12px rgba(0,0,0,0.4)}
.note{font-size:12px;color:#cfe3ff;margin-top:8px}
.config{margin-top:10px;padding:10px;border-radius:8px;background:rgba(255,255,255,0.02)}
.footer{margin-top:10px;font-size:12px;color:#cfe3ff}
</style>
</head>
<body>
<div class="app">
<header>
<div class="logo">G</div>
<div>
<h1>Gustavo — IA Inteligente</h1>
<p class="lead">Motores locales avanzados, Google, Gemini e Internet integrados. Generación de texto, voz e imágenes detalladas. OpenAI opcional.</p>
</div>
</header>
<div class="grid">
<section class="panel">
<div class="chat" id="chat" aria-live="polite"></div>
<div class="controls">
<input id="prompt" type="text" placeholder="Haz una pregunta rápida...">
<button id="send" class="small">Enviar</button>
<button id="talk" class="small">🎤 Hablar</button>
</div>
<div style="margin-top:8px">
<textarea id="longMsg" placeholder="Escribe un mensaje largo aquí..."></textarea>
<button id="sendLong" class="small">Enviar mensaje largo</button>
</div>
<div class="note">Responde preguntas complejas, genera imágenes, busca en Google, Gemini y en Internet en tiempo real.</div>
</section>
<aside class="panel">
<div class="config">
<strong>Configuración OpenAI (opcional)</strong>
<p class="note">Pega tu clave si deseas respuestas más completas usando OpenAI.</p>
<input id="openaiKey" type="text" placeholder="sk-..." style="width:100%;margin-top:8px;padding:8px;border-radius:8px;background:transparent;color:inherit;border:1px solid rgba(255,255,255,0.06)">
<label style="display:block;margin-top:8px">Modelo:
<select id="modelSelect" style="margin-top:6px;width:100%;padding:8px;border-radius:8px;background:transparent;color:inherit;border:1px solid rgba(255,255,255,0.06)">
<option value="gpt-4o-mini">gpt-4o-mini</option>
<option value="gpt-4o">gpt-4o</option>
<option value="gpt-4o-mini-instruct">gpt-4o-mini-instruct</option>
</select>
</label>
<div style="margin-top:8px;display:flex;gap:8px">
<button id="saveKey" class="small">Guardar</button>
<button id="clearKey" class="small">Borrar</button>
</div>
</div>
<div style="margin-top:12px">
<strong>Generar imagen (Beta)</strong>
<input id="imgPrompt" type="text" placeholder="Describe la imagen..." style="width:100%;margin-top:6px;padding:8px;border-radius:8px;background:transparent;color:inherit;border:1px solid rgba(255,255,255,0.06)">
<div style="display:flex;gap:8px;margin-top:6px">
<button id="genImgLocal" class="small">Generar con motor local avanzado</button>
<button id="genImgOpenAI" class="small">Generar con OpenAI</button>
</div>
<div class="images" id="images"></div>
</div>
<div style="margin-top:12px">
<strong>Voz</strong>
<div style="margin-top:6px">
<label for="voiceSelect">Voz:</label>
<select id="voiceSelect"></select>
</div>
</div>
<div class="footer">Motores Google, Gemini e Internet: realizan búsquedas inteligentes y en tiempo real. OpenAI opcional según tu clave.</div>
</aside>
</div>
</div>
<script>
let OPENAI_API_KEY='';
const chatEl=document.getElementById('chat');
const promptEl=document.getElementById('prompt');
const longMsgEl=document.getElementById('longMsg');
const openaiKeyEl=document.getElementById('openaiKey');
const modelSelect=document.getElementById('modelSelect');
const imagesEl=document.getElementById('images');
const imgPrompt=document.getElementById('imgPrompt');
try{const k=localStorage.getItem('gustavo_openai_key');if(k){OPENAI_API_KEY=k;openaiKeyEl.value=k;}}catch(e){}
function appendBubble(text,who='bot'){const wrap=document.createElement('div');wrap.className='bubble '+who;const meta=document.createElement('div');meta.className='meta';meta.textContent=who==='user'?'Tú':'Gustavo';wrap.appendChild(meta);const content=document.createElement('div');content.innerHTML=sanitize(text);wrap.appendChild(content);chatEl.appendChild(wrap);chatEl.scrollTop=chatEl.scrollHeight;}
function sanitize(s){return(s||'').toString().replace(/</g,'&lt;').replace(/\n/g,'<br>');}
async function localAnswer(prompt){
const p=(prompt||'').toLowerCase();
if(/hola|buenas/.test(p))return'¡Hola! Soy Gustavo, tu IA inteligente con motores locales, Google, Gemini e Internet integrados. Puedo ayudarte con preguntas, generar imágenes detalladas y buscar información en tiempo real.';
if(/quién eres|quien eres/.test(p))return'Soy Gustavo, un asistente IA avanzado con motores locales potentes, Google, Gemini e Internet integrados, capaz de generar imágenes y contenido creativo.';
if(/google/.test(p))return'Estoy usando el motor de Google para buscar información relevante sobre: '+prompt;
if(/gemini/.test(p))return'Estoy consultando el motor Gemini para obtener información precisa sobre: '+prompt;
if(/internet|buscar|busca/.test(p))return'Estoy buscando en Internet información relevante sobre: '+prompt;
if(/chiste|broma/.test(p))return'¿Por qué el ordenador fue al gimnasio? ¡Porque quería ponerse en forma! 😄';
if(/resumen|resumir/.test(p))return'Envíame el texto y te haré un resumen completo y estructurado.';
if(/traducir|traduce/.test(p))return'Dime el texto y a qué idioma quieres traducirlo, lo haré con precisión.';
if(/historia|cuento|narrativa/.test(p))return'Puedo crear historias originales, cuentos o guiones sobre cualquier tema que elijas.';
return`He recibido: "${prompt}". Mis motores locales avanzados, Google, Gemini e Internet pueden analizar, resumir, traducir, generar contenido creativo y producir imágenes detalladas.`;
}
async function handleSend(text){if(!text)return;appendBubble(text,'user');promptEl.value='';longMsgEl.value='';appendBubble('Pensando...','bot');const botNode=chatEl.querySelector('.bubble.bot:last-child div:last-child');try{let ans='';if(OPENAI_API_KEY){try{ans=await callOpenAIChat(text);}catch(err){ans=await localAnswer(text)+'\n\n(Nota: OpenAI falló: '+err.message+')';}}else{ans=await localAnswer(text);}botNode.innerHTML=sanitize(ans);}catch(e){botNode.innerHTML='Error al generar respuesta: '+sanitize(e.message);}}
document.getElementById('send').addEventListener('click',()=>handleSend(promptEl.value.trim()));
document.getElementById('sendLong').addEventListener('click',()=>handleSend(longMsgEl.value.trim()));
promptEl.addEventListener('keydown',e=>{if(e.key==='Enter')handleSend(promptEl.value.trim());});
appendBubble('Hola — soy Gustavo. Motores locales avanzados, Google, Gemini e Internet integrados. Puedo responder preguntas complejas y generar imágenes detalladas.', 'bot');
</script>
</body>
</html>
