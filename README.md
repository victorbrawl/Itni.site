<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Itini</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Tahoma, sans-serif; }
    body { background-color: #f5f5f5; }

    .hidden { display: none; }

    .container {
      max-width: 400px;
      margin: 100px auto;
      padding: 30px;
      background: white;
      border-radius: 12px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.1);
    }

    h2, h1 { text-align: center; margin-bottom: 20px; }

    label { font-weight: bold; margin-top: 10px; display: block; }
    input { width: 100%; padding: 10px; margin-top: 5px; margin-bottom: 15px; border-radius: 8px; border: 1px solid #ccc; }
    button { width: 100%; padding: 12px; background-color: #4a90e2; color: white; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; }
    button:hover { background-color: #357bd8; }

    .admin-buttons { display: flex; gap: 10px; margin-top: 10px; }
    .admin-buttons button { flex: 1; padding: 10px; background-color: #555; }
    .admin-buttons button:hover { background-color: #333; }

    .chatgpt-layout { display: flex; flex-direction: column; height: 100vh; }
    .chatgpt-header { background: #202123; color: white; padding: 15px; font-size: 20px; font-weight: bold; }
    .chatgpt-main { flex: 1; overflow-y: auto; padding: 20px; background: #343541; color: white; }
    .chatgpt-input { padding: 15px; background: #202123; display: flex; gap: 10px; }
    .chatgpt-input input { flex: 1; padding: 12px; border-radius: 8px; border: none; }
    .chatgpt-input button { width: 100px; border-radius: 8px; background: #4a90e2; }
    .user-msg { color: #2e90fa; margin-bottom: 10px; }
    .bot-msg { color: #a78bfa; margin-bottom: 10px; }
  </style>
</head>
<body>

<!-- Cadastro -->
<div class="container" id="cadastroTela">
  <h2>Criar Conta</h2>
  <label>Nome</label><input type="text" id="nome" required>
  <label>E-mail</label><input type="email" id="email" required>
  <label>Senha</label><input type="password" id="senha" required>
  <label>Confirmar Senha</label><input type="password" id="confirmar" required>
  <button onclick="fazerCadastro()">Cadastrar</button>
  
  <div class="admin-buttons">
    <button onclick="mostrarAdmin('victortop')">Admin</button>
    <button onclick="mostrarAdmin('victortop2')">Hiper Admin</button>
  </div>
</div>

<!-- Administrador -->
<div class="container hidden" id="adminTela">
  <h2>Login</h2>
  <label>Senha</label><input type="password" id="adminSenha">
  <button onclick="verificarAdmin()">Entrar</button>
</div>

<!-- Modo Plus -->
<div class="container hidden" id="plusTela">
  <h2>Itini Plus</h2>
  <div id="chatPlus" style="height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; margin-bottom: 10px; border-radius: 8px;"></div>
  <input type="text" id="inputPlus" placeholder="Digite sua pergunta...">
  <button onclick="enviarMensagemPlus()">Enviar</button>
  <button onclick="mostrarHiperPlus()">Modo Hiper Plus</button>
</div>

<script>
  let acessoDireto = '';

  function fazerCadastro() {
    const senha = document.getElementById('senha').value;
    const confirmar = document.getElementById('confirmar').value;
    if (senha !== confirmar) return alert('Senhas não coincidem!');
    document.getElementById('cadastroTela').classList.add('hidden');
    document.getElementById('plusTela').classList.remove('hidden');
  }

  function mostrarAdmin(tipo) {
    acessoDireto = tipo;
    document.getElementById('cadastroTela').classList.add('hidden');
    document.getElementById('adminTela').classList.remove('hidden');
  }

  function verificarAdmin() {
    const senha = document.getElementById('adminSenha').value;
    if (senha === acessoDireto) {
      document.getElementById('adminTela').classList.add('hidden');
      if (senha === 'victortop') {
        document.getElementById('plusTela').classList.remove('hidden');
      } else if (senha === 'victortop2') {
        abrirHiperPlus();
      }
    } else {
      alert('Senha incorreta!');
    }
  }

  function mostrarHiperPlus() {
    const senha = prompt('Senha do Modo Hiper Plus:');
    if (senha === 'victortop2') {
      document.getElementById('plusTela').classList.add('hidden');
      abrirHiperPlus();
    } else {
      alert('Senha incorreta!');
    }
  }

  function abrirHiperPlus() {
    const hiperDiv = document.createElement('div');
    hiperDiv.className = 'chatgpt-layout';
    hiperDiv.innerHTML = `
      <div class="chatgpt-header">Itini Hiper Plus</div>
      <div id="hiperChat" class="chatgpt-main"></div>
      <div class="chatgpt-input">
        <input type="text" id="inputHiper" placeholder="Escreva aqui...">
        <button onclick="enviarMensagemHiper()">Enviar</button>
      </div>
    `;
    document.body.innerHTML = '';
    document.body.appendChild(hiperDiv);
  }

  function enviarMensagemPlus() {
    const input = document.getElementById('inputPlus');
    const chat = document.getElementById('chatPlus');
    const msg = input.value.trim();
    if (!msg) return;
    chat.innerHTML += `<div><strong>Você:</strong> ${msg}</div>`;
    let resposta = '';
    const mensagem = msg.toLowerCase().normalize("NFD").replace(/[\u0300-\u036f]/g, "");

    if (mensagem.includes('voce e legal') || mensagem.includes('voce e top')) {
      resposta = 'Obrigado! Você também é incrível!';
    } else {
      resposta = 'Modo Plus respondeu: ' + msg;
    }

    chat.innerHTML += `<div><strong>Itini:</strong> ${resposta}</div>`;
    input.value = '';
    chat.scrollTop = chat.scrollHeight;
  }

  function enviarMensagemHiper() {
    const input = document.getElementById('inputHiper');
    const chat = document.getElementById('hiperChat');
    const msg = input.value.trim();
    if (!msg) return;

    const userMsg = document.createElement('div');
    userMsg.className = 'user-msg';
    userMsg.textContent = `Você: ${msg}`;
    chat.appendChild(userMsg);

    let resposta = '';
    if (msg.toLowerCase().includes('quem criou')) {
      resposta = 'Fui criada pelo Victor!';
    } else {
      resposta = msg;
    }

    const botMsg = document.createElement('div');
    botMsg.className = 'bot-msg';
    botMsg.textContent = `Itini: ${resposta}`;
    chat.appendChild(botMsg);

    input.value = '';
    chat.scrollTo({ top: chat.scrollHeight, behavior: 'smooth' });
  }
</script>

</body>
</html>0
