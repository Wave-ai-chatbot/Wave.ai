<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Wave.ai — Chatbot</title>
  <style>
    :root{--bg:#18df43;--card:#0c52dd;--accent:#2dd4bf;--muted:#94a3b8;--msg:#111827}
    html,body{height:100%;margin:0;font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,"Helvetica Neue",Arial}
    body{background:linear-gradient(180deg,#071029 0%, #071827 100%);display:flex;align-items:center;justify-content:center;padding:24px;color:#e6eef6}
    .app{width:100%;max-width:920px;background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0.01));border-radius:12px;box-shadow:0 8px 30px rgba(2,6,23,0.7);overflow:hidden;display:grid;grid-template-columns:320px 1fr}
    .sidebar{padding:18px;background:rgba(12, 69, 212, 0.6);border-right:1px solid rgba(198, 211, 21, 0.03)}
    .brand{display:flex;gap:12px;align-items:center;margin-bottom:14px}
    .logo{width:46px;height:46px;border-radius:10px;background:linear-gradient(135deg,var(--accent),#0284c7);display:flex;align-items:center;justify-content:center;font-weight:800;color:#07203a}
    .title{font-size:16px;font-weight:700}
    .subtitle{font-size:12px;color:var(--muted)}
    .convos{margin-top:12px;display:flex;flex-direction:column;gap:8px}
    .convo{padding:10px;border-radius:8px;background:rgba(255,255,255,0.01);cursor:pointer;font-size:13px;color:var(--muted);display:flex;justify-content:space-between}
    .convo.active{background:linear-gradient(90deg,rgba(45,212,191,0.08),rgba(2,132,199,0.04));color:#dffbf6}
    .main{display:flex;flex-direction:column;height:600px}
    .chat-header{padding:16px;border-bottom:1px solid rgba(255,255,255,0.03);display:flex;align-items:center;justify-content:space-between}
    .chat-area{flex:1;overflow:auto;padding:20px;display:flex;flex-direction:column;gap:12px;background:linear-gradient(180deg, rgba(10,15,25,0.02), transparent)}
    .msg{max-width:78%;padding:12px;border-radius:12px;font-size:14px;line-height:1.35}
    .msg.user{align-self:flex-end;background:linear-gradient(180deg,#065f46,#047857);color:white;border-bottom-right-radius:4px}
    .msg.bot{align-self:flex-start;background:linear-gradient(180deg,#0b1220,#0f1724);border:1px solid rgba(255,255,255,0.03);color:#d7eef0}
    .input-area{padding:14px;border-top:1px solid rgba(255,255,255,0.03);display:flex;gap:8px}
    .input-area textarea{flex:1;resize:none;height:48px;padding:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.03);background:transparent;color:inherit}
    .btn{background:var(--accent);border:none;padding:10px 14px;border-radius:10px;color:#042023;font-weight:700;cursor:pointer}
    .muted{color:var(--muted);font-size:13px}
    .small{font-size:12px}
    .controls{display:flex;gap:8px;align-items:center}
    .switch{display:flex;gap:6px;align-items:center}
    .chip{padding:8px;border-radius:999px;background:rgba(255,255,255,0.02);font-size:12px}
    .typing{width:46px;height:8px;border-radius:999px;background:linear-gradient(90deg,#c9fff6 0%, #2dd4bf 50%, #c9fff6 100%);animation: shimmer 1.1s linear infinite}
    @keyframes shimmer{0%{transform:translateX(-40px)}100%{transform:translateX(40px)}}
    footer.info{padding:10px 16px;border-top:1px solid rgba(255,255,255,0.02);font-size:12px;color:var(--muted)}
    .hint{font-size:12px;color:var(--muted);margin-top:6px}
  </style>
</head>
<body>
  <!--
    Wave.ai — Single-file demo chat UIHow to use:
  - This file is a frontend demo. For a real AI you must provide a backend that calls OpenAI (or another model) and exposes an endpoint such as /api/chat.
  - By default this demo runs in MOCK mode so you can try the UI without any API keys. To enable real AI mode, set `APP_MODE = 'server'` in the script below and implement the server API described in the comments.

Server API (recommended):
  POST /api/chat with JSON { "message": "user message", "history": [ {role:'user'|'assistant', content: '...'} ] }
  Responds with { "reply": "Assistant reply text" }

IMPORTANT SECURITY NOTE:
  Do NOT put your OpenAI API key into client-side JavaScript for production — that would expose your key. Always proxy requests through your server.

-->

  <div class="app" role="application" aria-label="Wave.ai chat app">
    <aside class="sidebar">
      <div class="brand">
        <div class="logo">W</div>
        <div>
          <div class="title">Wave.ai</div>
          <div class="subtitle">Your friendly assistant</div>
        </div>
      </div><div class="controls">
    <div class="chip" id="modeChip">Mode: MOCK</div>
    <div class="chip small" id="clearBtn" style="cursor:pointer">New Chat</div>
  </div>

  <div class="hint">Conversations</div>
  <div class="convos" id="convos"></div>

  <div style="margin-top:12px" class="muted">Local demo — no keys required</div>
</aside>

<main class="main">
  <div class="chat-header">
    <div>
      <div style="font-weight:800">Wave.ai</div>
      <div class="muted small">Assistant</div>
    </div>
    <div class="muted small">Wave.ai · demo</div>
  </div>

  <div class="chat-area" id="chatArea" aria-live="polite"></div>

  <div class="input-area">
    <textarea id="input" placeholder="Type your message and press Enter" aria-label="Message input"></textarea>
    <button class="btn" id="send">Send</button>
  </div>

  <footer class="info">Wave.ai demo • Responses may be simulated in MOCK mode.</footer>
</main>

  </div> <script>
    // ========== CONFIG ===========
    // APP_MODE: 'mock' | 'server'
    // - 'mock' uses a fast deterministic local reply generator (no API key needed)
    // - 'server' calls a backend endpoint at POST /api/chat and expects JSON { reply: '...' }
    const APP_MODE = 'mock';
    const API_ENDPOINT = '/api/chat'; // change this to your serverless function if needed

    // Bot identity
    const BOT_NAME = 'Wave';

    // ========== END CONFIG =======

    // Simple in-memory conversation store, also persisted to localStorage
    const state = {
      chats: [], // each chat: {id, title, messages: [{role,content}]}
      active: null
    };

    // Utilities
    const $ = id => document.getElementById(id);
    const save = () => localStorage.setItem('waveai_state_v1', JSON.stringify(state));
    const load = () => {
      try{const s = JSON.parse(localStorage.getItem('waveai_state_v1')||'null'); if(s){state.chats=s.chats;state.active=s.active}}catch(e){}
    }

    // Mock responder — simple heuristics + small canned replies
    function mockReply(user, history){
      const text = user.toLowerCase();
      if(text.includes('hello')||text.includes('hi')) return `Hello! I'm ${BOT_NAME}. How can I help you today?`;
      if(text.includes('time')) return `It's currently ${new Date().toLocaleString()}.`; 
      if(text.includes('joke')) return `Why did the developer go broke? Because he used up all his cache. 😄`;
      if(text.includes('help')) return `Sure! I'm here to assist you. What do you need help with? You can try:- asking for a joke, the time, the weather, a motivational quote, or an inspiration. `;
      if(text.includes('weather')) return `I don't have real-time weather data, but I hope it's nice where you are!`;
      if(text.includes("your name")) return `I'm ${BOT_NAME}, your friendly AI assistant!`;
      if(text.includes('thank')||text.includes('thanks')) return `You're welcome! If you have any more questions, feel free to ask.`;
      if(text.includes('goodbye')||text.includes('bye')) return `Goodbye! Have a great day!`;
      if(text.includes('who are you')) return `I'm ${BOT_NAME}, an AI assistant here to help you with various tasks.`;
      if(text.includes('what can you do')) return `I can chat with you, tell jokes, provide the time, and more! Just ask.`;
      if(text.includes('good morning')) return `Good morning! Hope you have a fantastic day ahead.`;
      if(text.includes('good night')) return `Good night! Sleep well and sweet dreams.`;
      if(text.includes('motivate me')) return `Believe in yourself! Every day is a new opportunity to achieve your goals. You've got this!`;
      if(text.includes('inspire me')) return `The only way to do great work is to love what you do. - Steve Jobs`;
      if(text.includes('quote')) return `“The best way to predict the future is to invent it.” Alan Kay`;
      if(text.includes('good')) return `I'm glad to hear that! How can I assist you further?`;
      if(text.length < 10) return `Sorry but I can't help with that. You can try:- asking for a joke, the time, the weather, a motivational quote, or an inspiration.`;
      // fallback: echo with small transformation
      return `You said: "${user}" — Sorry but I can't help with that. You can try:- asking for a joke, the time, the weather, a motivational quote, or an inspiration.`;
    }

    // Create a new chat
    function newChat(title){
      const chat = {id:Date.now().toString(), title: title||'New chat', messages: []};
      state.chats.unshift(chat);
      state.active = chat.id;
      save();
      renderConvos();
      renderActive();
    }

    function deleteChat(id){
      state.chats = state.chats.filter(c=>c.id!==id);
      if(state.active===id){ state.active = state.chats[0]?.id || null }
      save(); renderConvos(); renderActive();
    }

    function setActive(id){ state.active = id; save(); renderConvos(); renderActive(); }

    // UI render
    function renderConvos(){
      const c = $('convos'); c.innerHTML='';
      state.chats.forEach(chat=>{
        const el = document.createElement('div'); el.className='convo'+(chat.id===state.active?' active':'');
        el.tabIndex=0;
        el.innerHTML = `<div style="flex:1">${escapeHtml(chat.title)}</div><div style="margin-left:8px;color:var(--muted);font-size:12px;display:flex;gap:6px"><span style='cursor:pointer' data-id='${chat.id}' class='del'>Del</span></div>`;
        el.addEventListener('click', ()=> setActive(chat.id));
        c.appendChild(el);
      });
    }

    function renderActive(){
      const area = $('chatArea'); area.innerHTML='';
      const active = state.chats.find(c=>c.id===state.active);
      if(!active){ area.innerHTML = '<div class="muted">No conversation. Click "New Chat" to start.</div>'; return }
      // Title show
      // show messages
      active.messages.forEach(m=>{
        const d = document.createElement('div'); d.className='msg '+(m.role==='user'?'user':'bot'); d.textContent = m.content; area.appendChild(d);
      });
      area.scrollTop = area.scrollHeight;
    }

    function addMessage(role, text){
      const chat = state.chats.find(c=>c.id===state.active);
      if(!chat) return;
      chat.messages.push({role,content:text});
      // update title if first user message
      if(!chat.title || chat.title==='New chat'){ chat.title = text.slice(0,40) }
      save(); renderConvos(); renderActive();
    }

    // Send workflow
    async function sendMessage(){
      const input = $('input'); const text = input.value.trim(); if(!text) return;
      addMessage('user', text);
      input.value='';
      // show typing indicator
      const area = $('chatArea');
      const typingEl = document.createElement('div'); typingEl.className='msg bot'; typingEl.id='typing'; typingEl.innerHTML = '<div class="typing" style="width:120px"></div>'; area.appendChild(typingEl); area.scrollTop = area.scrollHeight;

      try{
        let reply;
        if(APP_MODE==='mock'){
          // simulate latency
          await new Promise(r=>setTimeout(r, 350 + Math.random()*450));
          reply = mockReply(text, null);
        }else{
          // call your server backend
          const res = await fetch(API_ENDPOINT, {
            method:'POST', headers:{'Content-Type':'application/json'}, body: JSON.stringify({message: text, history: state.chats.find(c=>c.id===state.active).messages})
          });
          if(!res.ok){ throw new Error('Server error ' + res.status) }
          const data = await res.json();
          reply = data.reply || data.choices?.[0]?.message?.content || 'Sorry — no reply.';
        }

        // replace typing with reply
        const t = document.getElementById('typing'); if(t) t.remove();
        addMessage('assistant', reply);
      }catch(err){
        const t = document.getElementById('typing'); if(t) t.remove();
        addMessage('assistant', `Error: ${err.message}`);
      }
    }

    // Helpers
    function escapeHtml(s){return s.replace(/[&<>"']/g, c=>({"&":"&amp;","<":"&lt;",">":"&gt;","\"":"&quot;","'":"&#39;"})[c])}

    // Init
    (function(){
      load(); // load persisted state
      // ensure at least one chat
      if(!state.chats || state.chats.length===0){ newChat('New chat'); }
      // wire UI
      $('send').addEventListener('click', sendMessage);
      $('input').addEventListener('keydown', (e)=>{ if(e.key==='Enter' && !e.shiftKey){ e.preventDefault(); sendMessage(); } });
      $('clearBtn').addEventListener('click', ()=>{
        newChat('New chat');
      });
      // mode chip
      const modeChip = $('modeChip');
      modeChip.textContent = 'Mode: '+ (APP_MODE==='mock' ? 'MOCK' : 'SERVER');
      // initial render
      renderConvos(); renderActive();
    })();
  </script></body>
</html>
