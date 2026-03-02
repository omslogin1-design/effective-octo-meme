<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>StreamPortal — Portal do Cliente</title>
  <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Syne:wght@400;600;700;800&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
  <style>
    :root {
      --red: #E50914;
      --red-d: #b20710;
      --bg: #080808;
      --s1: #111;
      --s2: #1a1a1a;
      --s3: #222;
      --brd: #2c2c2c;
      --txt: #f2f2f2;
      --mut: #777;
      --grn: #00e676;
      --ylw: #ffb300;
      --blu: #2979ff;
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      background: var(--bg);
      color: var(--txt);
      font-family: 'Syne', sans-serif;
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* NOISE EFFECT */
    body::after {
      content: '';
      position: fixed;
      inset: 0;
      pointer-events: none;
      z-index: 9999;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
      opacity: .4;
    }

    /* PAGE STRUCTURE */
    .page {
      display: none;
      min-height: 100vh;
      flex-direction: column;
      animation: fadeIn 0.4s ease-in;
    }

    .page.active {
      display: flex;
    }

    @keyframes fadeIn {
      from {
        opacity: 0;
      }
      to {
        opacity: 1;
      }
    }

    /* LOGIN PAGE */
    #pg-login {
      align-items: center;
      justify-content: center;
      background: radial-gradient(ellipse 80% 60% at 20% 0%, #1c0003 0%, var(--bg) 65%);
    }

    #pg-adm-login {
      align-items: center;
      justify-content: center;
      background: radial-gradient(ellipse 80% 60% at 80% 100%, #001c1c 0%, var(--bg) 65%);
    }

    .login-wrap {
      width: 420px;
      animation: riseIn 0.5s ease both;
    }

    .login-logo {
      font-family: 'Bebas Neue', sans-serif;
      font-size: 3rem;
      letter-spacing: 4px;
      color: var(--red);
      text-align: center;
    }

    .login-logo span {
      color: var(--txt);
    }

    #pg-adm-login .login-logo {
      color: var(--blu);
    }

    .login-tagline {
      color: var(--mut);
      font-size: 0.82rem;
      text-align: center;
      margin-bottom: 30px;
    }

    .login-box {
      background: rgba(17, 17, 17, .95);
      border: 1px solid var(--brd);
      border-radius: 8px;
      padding: 40px 36px;
      backdrop-filter: blur(20px);
    }

    .fld {
      margin-bottom: 18px;
    }

    .fld label {
      display: block;
      font-size: 0.72rem;
      color: var(--mut);
      text-transform: uppercase;
      margin-bottom: 7px;
      font-weight: 600;
    }

    .fld input,
    .fld select {
      width: 100%;
      background: var(--s2);
      border: 1px solid var(--brd);
      border-radius: 5px;
      padding: 13px 15px;
      color: var(--txt);
      font-family: 'DM Mono', monospace;
      outline: none;
      transition: border-color 0.3s ease;
    }

    .fld input:focus,
    .fld select:focus {
      border-color: var(--red);
      box-shadow: 0 0 12px rgba(229, 9, 20, 0.2);
    }

    .btn-main {
      width: 100%;
      background: var(--red);
      border: none;
      border-radius: 5px;
      padding: 14px;
      color: #fff;
      font-weight: 700;
      cursor: pointer;
      margin-top: 8px;
      transition: background-color 0.3s ease, transform 0.1s ease;
      font-size: 1rem;
    }

    .btn-main:hover {
      background: var(--red-d);
      transform: translateY(-2px);
    }

    .btn-main:active {
      transform: translateY(0);
    }

    .err-box {
      background: rgba(229, 9, 20, .1);
      border: 1px solid rgba(229, 9, 20, .25);
      padding: 11px;
      font-size: 0.83rem;
      color: #ff6b6b;
      margin-top: 14px;
      display: none;
      border-radius: 5px;
      animation: slideDown 0.3s ease;
    }

    .err-box.show {
      display: block;
    }

    @keyframes slideDown {
      from {
        opacity: 0;
        transform: translateY(-10px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .login-foot {
      text-align: center;
      margin-top: 20px;
    }

    .login-foot a {
      color: var(--mut);
      font-size: 0.8rem;
      text-decoration: none;
      transition: color 0.3s ease;
    }

    .login-foot a:hover {
      color: var(--txt);
    }

    /* TOPBAR */
    .topbar {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      z-index: 200;
      background: rgba(8, 8, 8, .94);
      border-bottom: 1px solid var(--brd);
      backdrop-filter: blur(16px);
      height: 58px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 28px;
    }

    .topbar-title {
      font-family: 'Bebas Neue', sans-serif;
      font-size: 1.5rem;
      letter-spacing: 2px;
    }

    .topbar-title span {
      color: var(--red);
    }

    /* DASHBOARD & ADMIN */
    #pg-dash,
    #pg-admin {
      flex-direction: column;
      padding-top: 58px;
    }

    .dash-inner,
    .admin-inner {
      padding: 32px;
      max-width: 1080px;
      margin: 0 auto;
      width: 100%;
      flex: 1;
    }

    .dash-greet h1 {
      font-family: 'Bebas Neue', sans-serif;
      font-size: 2.8rem;
      letter-spacing: 3px;
      margin-bottom: 10px;
    }

    .dash-greet h1 span {
      color: var(--red);
    }

    /* KPI CARDS */
    .kpi-row {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 14px;
      margin: 25px 0;
    }

    .kpi {
      background: var(--s1);
      border: 1px solid var(--brd);
      border-radius: 8px;
      padding: 22px 20px;
      position: relative;
      overflow: hidden;
      transition: border-color 0.3s ease, transform 0.3s ease;
    }

    .kpi:hover {
      border-color: var(--red);
      transform: translateY(-4px);
    }

    .kpi::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      height: 3px;
      background: var(--red);
    }

    .kpi.g::before {
      background: var(--grn);
    }

    .kpi-lbl {
      font-size: 0.7rem;
      text-transform: uppercase;
      color: var(--mut);
      margin-bottom: 10px;
      letter-spacing: 1px;
    }

    .kpi-val {
      font-family: 'Bebas Neue', sans-serif;
      font-size: 1.9rem;
      font-weight: 700;
    }

    /* PIX SECTION */
    .pix-section {
      background: linear-gradient(135deg, var(--s1) 0%, rgba(0, 230, 118, .04) 100%);
      border: 1px solid var(--brd);
      border-radius: 8px;
      padding: 28px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 24px;
      margin-bottom: 26px;
      flex-wrap: wrap;
    }

    .pix-section h3 {
      margin: 0;
      font-size: 1.3rem;
    }

    .pix-section p {
      margin: 5px 0 0 0;
    }

    .btn-pix {
      background: var(--grn);
      border: none;
      border-radius: 5px;
      padding: 13px 24px;
      font-weight: 800;
      cursor: pointer;
      color: #000;
      transition: background-color 0.3s ease, transform 0.1s ease;
      font-size: 0.95rem;
    }

    .btn-pix:hover {
      background: #00cc5e;
      transform: translateY(-2px);
    }

    /* MODAL */
    .modal-bg {
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, .85);
      z-index: 500;
      display: none;
      align-items: center;
      justify-content: center;
      backdrop-filter: blur(8px);
      animation: fadeIn 0.3s ease;
    }

    .modal-bg.open {
      display: flex;
    }

    .modal-box {
      background: var(--s1);
      border: 1px solid var(--brd);
      border-radius: 12px;
      padding: 36px;
      width: 400px;
      text-align: center;
      max-width: 90vw;
      animation: scaleIn 0.3s ease;
    }

    @keyframes scaleIn {
      from {
        opacity: 0;
        transform: scale(0.9);
      }
      to {
        opacity: 1;
        transform: scale(1);
      }
    }

    .modal-box h2 {
      margin-bottom: 20px;
    }

    /* ADMIN TABS */
    .tabs {
      display: flex;
      border-bottom: 1px solid var(--brd);
      margin-bottom: 20px;
      flex-wrap: wrap;
    }

    .tab-btn {
      background: none;
      border: none;
      color: var(--mut);
      padding: 12px 20px;
      cursor: pointer;
      border-bottom: 2px solid transparent;
      transition: color 0.3s ease, border-color 0.3s ease;
      font-weight: 500;
    }

    .tab-btn:hover {
      color: var(--txt);
    }

    .tab-btn.on {
      color: var(--red);
      border-bottom-color: var(--red);
    }

    .tab-pane {
      display: none;
      animation: fadeIn 0.3s ease;
    }

    .tab-pane.on {
      display: block;
    }

    .adm-form {
      background: var(--s1);
      padding: 20px;
      border-radius: 8px;
      border: 1px solid var(--brd);
    }

    .fg {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
      gap: 15px;
      margin-bottom: 20px;
    }

    /* TABLE */
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 10px;
      overflow-x: auto;
    }

    thead {
      background: #151515;
    }

    th {
      text-align: left;
      font-size: 10px;
      color: var(--mut);
      padding: 12px 10px;
      text-transform: uppercase;
      letter-spacing: 1px;
      font-weight: 600;
    }

    td {
      padding: 12px 10px;
      border-bottom: 1px solid #222;
      font-size: 13px;
    }

    tr:hover {
      background: rgba(229, 9, 20, 0.05);
    }

    /* BADGES */
    .badge {
      padding: 5px 10px;
      border-radius: 4px;
      font-size: 10px;
      font-weight: bold;
      text-transform: uppercase;
      display: inline-block;
      letter-spacing: 0.5px;
    }

    .badge.ativo {
      background: rgba(0, 230, 118, 0.15);
      color: var(--grn);
    }

    .badge.vencido {
      background: rgba(229, 9, 20, 0.15);
      color: var(--red);
    }

    /* ANIMATIONS */
    @keyframes riseIn {
      from {
        opacity: 0;
        transform: translateY(20px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    /* RESPONSIVE */
    @media (max-width: 768px) {
      .login-wrap {
        width: 90%;
      }

      .modal-box {
        width: 90%;
      }

      .pix-section {
        flex-direction: column;
        text-align: center;
      }

      .dash-greet h1 {
        font-size: 2rem;
      }

      .fg {
        grid-template-columns: 1fr;
      }

      table {
        font-size: 12px;
      }

      th, td {
        padding: 8px 5px;
      }
    }

    /* UTILITIES */
    .success-msg {
      background: rgba(0, 230, 118, .1);
      border: 1px solid rgba(0, 230, 118, .25);
      padding: 11px;
      font-size: 0.83rem;
      color: var(--grn);
      margin-top: 14px;
      display: none;
      border-radius: 5px;
    }

    .success-msg.show {
      display: block;
      animation: slideDown 0.3s ease;
    }
  </style>
</head>
<body>

  <!-- LOGIN PAGE -->
  <div id="pg-login" class="page active">
    <div class="login-wrap">
      <div class="login-logo">Stream<span>Portal</span></div>
      <p class="login-tagline">Acesso exclusivo para assinantes</p>
      <div class="login-box">
        <div class="fld">
          <label for="li-id">ID do Cliente</label>
          <input type="text" id="li-id" placeholder="Ex: CLI-001">
        </div>
        <div class="fld">
          <label for="li-pw">Senha</label>
          <input type="password" id="li-pw" placeholder="••••••••">
        </div>
        <button class="btn-main" onclick="doLogin()">ENTRAR NO PORTAL</button>
        <div class="err-box" id="li-err">❌ Acesso negado. Verifique os dados.</div>
      </div>
      <div class="login-foot">
        <a href="#" onclick="goPage('pg-adm-login'); return false;">Acesso Administrativo →</a>
      </div>
    </div>
  </div>

  <!-- ADMIN LOGIN PAGE -->
  <div id="pg-adm-login" class="page">
    <div class="login-wrap">
      <div class="login-logo">Painel<span>Admin</span></div>
      <div class="login-box">
        <div class="fld">
          <label for="adm-u">Usuário</label>
          <input type="text" id="adm-u" placeholder="Usuário">
        </div>
        <div class="fld">
          <label for="adm-p">Senha</label>
          <input type="password" id="adm-p" placeholder="••••••••">
        </div>
        <button class="btn-main" onclick="doAdmLogin()">ACESSAR PAINEL</button>
        <div class="err-box" id="adm-err">❌ Credenciais inválidas.</div>
        <div class="login-foot">
          <a href="#" onclick="goPage('pg-login'); return false;">← Voltar</a>
        </div>
      </div>
    </div>
  </div>

  <!-- CLIENT DASHBOARD -->
  <div id="pg-dash" class="page">
    <div class="topbar">
      <div class="topbar-title">Stream<span>Portal</span></div>
      <button class="btn-main" style="width: auto; padding: 6px 16px; font-size: 0.85rem;" onclick="doLogout()">Sair</button>
    </div>
    <div class="dash-inner">
      <div class="dash-greet">
        <h1>OLÁ, <span id="d-name">...</span> 👋</h1>
      </div>
      <div class="kpi-row">
        <div class="kpi">
          <div class="kpi-lbl">Status</div>
          <div id="d-status">...</div>
        </div>
        <div class="kpi">
          <div class="kpi-lbl">Vencimento</div>
          <div class="kpi-val" id="d-venc">...</div>
        </div>
        <div class="kpi g">
          <div class="kpi-lbl">Valor Mensal</div>
          <div class="kpi-val" id="d-valor">...</div>
        </div>
      </div>
      <div class="pix-section">
        <div>
          <h3>PAGAR MENSALIDADE</h3>
          <p>Gere sua chave PIX para renovação imediata.</p>
        </div>
        <button class="btn-pix" onclick="openPix()">⚡ GERAR PIX</button>
      </div>
    </div>
  </div>

  <!-- ADMIN PANEL -->
  <div id="pg-admin" class="page">
    <div class="topbar">
      <div class="topbar-title" style="color: var(--blu);">Painel<span style="color: var(--blu);">Admin</span></div>
      <button class="btn-main" style="width: auto; padding: 6px 16px; font-size: 0.85rem;" onclick="doLogout()">Sair</button>
    </div>
    <div class="admin-inner">
      <div class="tabs">
        <button class="tab-btn on" onclick="openTab(event, 't1')">+ Novo Cliente</button>
        <button class="tab-btn" onclick="openTab(event, 't2')">📋 Lista de Clientes</button>
      </div>

      <!-- TAB 1: NEW CLIENT FORM -->
      <div id="t1" class="tab-pane on">
        <div class="adm-form">
          <div class="fg">
            <div class="fld">
              <label for="f-id">ID</label>
              <input type="text" id="f-id" placeholder="CLI-001">
            </div>
            <div class="fld">
              <label for="f-nome">Nome</label>
              <input type="text" id="f-nome" placeholder="Nome do Cliente">
            </div>
            <div class="fld">
              <label for="f-pw">Senha</label>
              <input type="text" id="f-pw" placeholder="Senha" value="123">
            </div>
            <div class="fld">
              <label for="f-venc">Vencimento</label>
              <input type="date" id="f-venc">
            </div>
            <div class="fld">
              <label for="f-val">Valor Mensal</label>
              <input type="text" id="f-val" placeholder="29,90" value="29,90">
            </div>
            <div class="fld">
              <label for="f-status">Status</label>
              <select id="f-status">
                <option value="ativo">✅ Ativo</option>
                <option value="vencido">⏰ Vencido</option>
              </select>
            </div>
          </div>
          <button class="btn-main" onclick="saveClient()">💾 CADASTRAR CLIENTE</button>
          <div class="success-msg" id="save-success">✅ Cliente cadastrado com sucesso!</div>
        </div>
      </div>

      <!-- TAB 2: CLIENT LIST -->
      <div id="t2" class="tab-pane">
        <div style="overflow-x: auto;">
          <table>
            <thead>
              <tr>
                <th>ID</th>
                <th>NOME</th>
                <th>VENCIMENTO</th>
                <th>VALOR</th>
                <th>STATUS</th>
                <th>AÇÕES</th>
              </tr>
            </thead>
            <tbody id="lista-corpo">
              <tr>
                <td colspan="6" style="text-align: center; color: var(--mut);">Nenhum cliente cadastrado</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>

  <!-- PIX MODAL -->
  <div id="m-pix" class="modal-bg">
    <div class="modal-box">
      <h2 style="color: var(--grn); font-family: 'Bebas Neue'">🔑 CHAVE PIX</h2>
      <div id="pix-key" style="background: #000; padding: 15px; margin: 20px 0; font-family: monospace; font-size: 0.85rem; border: 1px dashed var(--grn); border-radius: 5px; word-break: break-all;">
        chave-exemplo@pix.com
      </div>
      <button class="btn-main" onclick="closePix()">FECHAR</button>
    </div>
  </div>

  <script>
    // ===== DATABASE =====
    const DB_KEY = 'streamportal_db_v2';
    
    let db = JSON.parse(localStorage.getItem(DB_KEY)) || {
      clientes: [
        {
          id: 'CLI-001',
          nome: 'João Silva',
          senha: '123',
          venc: '2026-12-31',
          valor: '29,90',
          status: 'ativo'
        }
      ],
      config: {
        pix: 'seuemail@pix.com'
      }
    };

    // Save database to localStorage
    function saveDB() {
      localStorage.setItem(DB_KEY, JSON.stringify(db));
    }

    // ===== NAVIGATION =====
    function goPage(pageId) {
      // Hide all pages
      document.querySelectorAll('.page').forEach(page => {
        page.classList.remove('active');
      });

      // Show target page
      const targetPage = document.getElementById(pageId);
      if (targetPage) {
        targetPage.classList.add('active');
      } else {
        console.error(`Página #${pageId} não encontrada`);
      }
    }

    // ===== LOGIN & LOGOUT =====
    function doLogin() {
      const clientId = document.getElementById('li-id').value.trim();
      const password = document.getElementById('li-pw').value.trim();
      const errorBox = document.getElementById('li-err');

      // Clear previous error
      errorBox.classList.remove('show');

      // Validate inputs
      if (!clientId || !password) {
        errorBox.innerText = '❌ Preencha todos os campos.';
        errorBox.classList.add('show');
        return;
      }

      // Find user
      const user = db.clientes.find(u => u.id === clientId && u.senha === password);

      if (user) {
        // Populate dashboard
        document.getElementById('d-name').innerText = user.nome;
        document.getElementById('d-status').innerHTML = `<span class="badge ${user.status}">${user.status.toUpperCase()}</span>`;
        document.getElementById('d-venc').innerText = formatDate(user.venc);
        document.getElementById('d-valor').innerText = 'R$ ' + user.valor;

        // Store current user in session
        sessionStorage.setItem('currentUser', JSON.stringify(user));

        // Navigate to dashboard
        goPage('pg-dash');

        // Clear form
        document.getElementById('li-id').value = '';
        document.getElementById('li-pw').value = '';
      } else {
        errorBox.innerText = '❌ ID ou senha incorretos.';
        errorBox.classList.add('show');
      }
    }

    function doAdmLogin() {
      const username = document.getElementById('adm-u').value.trim();
      const password = docum<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>StreamPortal — Portal do Cliente</title>
  <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Syne:wght@400;600;700;800&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
  <style>
    :root {
      --red: #E50914;
      --red-d: #b20710;
      --bg: #080808;
      --s1: #111;
      --s2: #1a1a1a;
      --s3: #222;
      --brd: #2c2c2c;
      --txt: #f2f2f2;
      --mut: #777;
      --grn: #00e676;
      --ylw: #ffb300;
      --blu: #2979ff;
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      background: var(--bg);
      color: var(--txt);
      font-family: 'Syne', sans-serif;
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* NOISE EFFECT */
    body::after {
      content: '';
      position: fixed;
      inset: 0;
      pointer-events: none;
      z-index: 9999;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
      opacity: .4;
    }

    /* PAGE STRUCTURE */
    .page {
      display: none;
      min-height: 100vh;
      flex-direction: column;
      animation: fadeIn 0.4s ease-in;
    }

    .page.active {
      display: flex;
    }

    @keyframes fadeIn {
      from {
        opacity: 0;
      }
      to {
        opacity: 1;
      }
    }

    /* LOGIN PAGE */
    #pg-login {
      align-items: center;
      justify-content: center;
      background: radial-gradient(ellipse 80% 60% at 20% 0%, #1c0003 0%, var(--bg) 65%);
    }

    #pg-adm-login {
      align-items: center;
      justify-content: center;
      background: radial-gradient(ellipse 80% 60% at 80% 100%, #001c1c 0%, var(--bg) 65%);
    }

    .login-wrap {
      width: 420px;
      animation: riseIn 0.5s ease both;
    }

    .login-logo {
      font-family: 'Bebas Neue', sans-serif;
      font-size: 3rem;
      letter-spacing: 4px;
      color: var(--red);
      text-align: center;
    }

    .login-logo span {
      color: var(--txt);
    }

    #pg-adm-login .login-logo {
      color: var(--blu);
    }

    .login-tagline {
      color: var(--mut);
      font-size: 0.82rem;
      text-align: center;
      margin-bottom: 30px;
    }

    .login-box {
      background: rgba(17, 17, 17, .95);
      border: 1px solid var(--brd);
      border-radius: 8px;
      padding: 40px 36px;
      backdrop-filter: blur(20px);
    }

    .fld {
      margin-bottom: 18px;
    }

    .fld label {
      display: block;
      font-size: 0.72rem;
      color: var(--mut);
      text-transform: uppercase;
      margin-bottom: 7px;
      font-weight: 600;
    }

    .fld input,
    .fld select {
      width: 100%;
      background: var(--s2);
      border: 1px solid var(--brd);
      border-radius: 5px;
      padding: 13px 15px;
      color: var(--txt);
      font-family: 'DM Mono', monospace;
      outline: none;
      transition: border-color 0.3s ease;
    }

    .fld input:focus,
    .fld select:focus {
      border-color: var(--red);
      box-shadow: 0 0 12px rgba(229, 9, 20, 0.2);
    }

    .btn-main {
      width: 100%;
      background: var(--red);
      border: none;
      border-radius: 5px;
      padding: 14px;
      color: #fff;
      font-weight: 700;
      cursor: pointer;
      margin-top: 8px;
      transition: background-color 0.3s ease, transform 0.1s ease;
      font-size: 1rem;
    }

    .btn-main:hover {
      background: var(--red-d);
      transform: translateY(-2px);
    }

    .btn-main:active {
      transform: translateY(0);
    }

    .err-box {
      background: rgba(229, 9, 20, .1);
      border: 1px solid rgba(229, 9, 20, .25);
      padding: 11px;
      font-size: 0.83rem;
      color: #ff6b6b;
      margin-top: 14px;
      display: none;
      border-radius: 5px;
      animation: slideDown 0.3s ease;
    }

    .err-box.show {
      display: block;
    }

    @keyframes slideDown {
      from {
        opacity: 0;
        transform: translateY(-10px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .login-foot {
      text-align: center;
      margin-top: 20px;
    }

    .login-foot a {
      color: var(--mut);
      font-size: 0.8rem;
      text-decoration: none;
      transition: color 0.3s ease;
    }

    .login-foot a:hover {
      color: var(--txt);
    }

    /* TOPBAR */
    .topbar {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      z-index: 200;
      background: rgba(8, 8, 8, .94);
      border-bottom: 1px solid var(--brd);
      backdrop-filter: blur(16px);
      height: 58px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 28px;
    }

    .topbar-title {
      font-family: 'Bebas Neue', sans-serif;
      font-size: 1.5rem;
      letter-spacing: 2px;
    }

    .topbar-title span {
      color: var(--red);
    }

    /* DASHBOARD & ADMIN */
    #pg-dash,
    #pg-admin {
      flex-direction: column;
      padding-top: 58px;
    }

    .dash-inner,
    .admin-inner {
      padding: 32px;
      max-width: 1080px;
      margin: 0 auto;
      width: 100%;
      flex: 1;
    }

    .dash-greet h1 {
      font-family: 'Bebas Neue', sans-serif;
      font-size: 2.8rem;
      letter-spacing: 3px;
      margin-bottom: 10px;
    }

    .dash-greet h1 span {
      color: var(--red);
    }

    /* KPI CARDS */
    .kpi-row {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 14px;
      margin: 25px 0;
    }

    .kpi {
      background: var(--s1);
      border: 1px solid var(--brd);
      border-radius: 8px;
      padding: 22px 20px;
      position: relative;
      overflow: hidden;
      transition: border-color 0.3s ease, transform 0.3s ease;
    }

    .kpi:hover {
      border-color: var(--red);
      transform: translateY(-4px);
    }

    .kpi::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      height: 3px;
      background: var(--red);
    }

    .kpi.g::before {
      background: var(--grn);
    }

    .kpi-lbl {
      font-size: 0.7rem;
      text-transform: uppercase;
      color: var(--mut);
      margin-bottom: 10px;
      letter-spacing: 1px;
    }

    .kpi-val {
      font-family: 'Bebas Neue', sans-serif;
      font-size: 1.9rem;
      font-weight: 700;
    }

    /* PIX SECTION */
    .pix-section {
      background: linear-gradient(135deg, var(--s1) 0%, rgba(0, 230, 118, .04) 100%);
      border: 1px solid var(--brd);
      border-radius: 8px;
      padding: 28px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 24px;
      margin-bottom: 26px;
      flex-wrap: wrap;
    }

    .pix-section h3 {
      margin: 0;
      font-size: 1.3rem;
    }

    .pix-section p {
      margin: 5px 0 0 0;
    }

    .btn-pix {
      background: var(--grn);
      border: none;
      border-radius: 5px;
      padding: 13px 24px;
      font-weight: 800;
      cursor: pointer;
      color: #000;
      transition: background-color 0.3s ease, transform 0.1s ease;
      font-size: 0.95rem;
    }

    .btn-pix:hover {
      background: #00cc5e;
      transform: translateY(-2px);
    }

    /* MODAL */
    .modal-bg {
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, .85);
      z-index: 500;
      display: none;
      align-items: center;
      justify-content: center;
      backdrop-filter: blur(8px);
      animation: fadeIn 0.3s ease;
    }

    .modal-bg.open {
      display: flex;
    }

    .modal-box {
      background: var(--s1);
      border: 1px solid var(--brd);
      border-radius: 12px;
      padding: 36px;
      width: 400px;
      text-align: center;
      max-width: 90vw;
      animation: scaleIn 0.3s ease;
    }

    @keyframes scaleIn {
      from {
        opacity: 0;
        transform: scale(0.9);
      }
      to {
        opacity: 1;
        transform: scale(1);
      }
    }

    .modal-box h2 {
      margin-bottom: 20px;
    }

    /* ADMIN TABS */
    .tabs {
      display: flex;
      border-bottom: 1px solid var(--brd);
      margin-bottom: 20px;
      flex-wrap: wrap;
    }

    .tab-btn {
      background: none;
      border: none;
      color: var(--mut);
      padding: 12px 20px;
      cursor: pointer;
      border-bottom: 2px solid transparent;
      transition: color 0.3s ease, border-color 0.3s ease;
      font-weight: 500;
    }

    .tab-btn:hover {
      color: var(--txt);
    }

    .tab-btn.on {
      color: var(--red);
      border-bottom-color: var(--red);
    }

    .tab-pane {
      display: none;
      animation: fadeIn 0.3s ease;
    }

    .tab-pane.on {
      display: block;
    }

    .adm-form {
      background: var(--s1);
      padding: 20px;
      border-radius: 8px;
      border: 1px solid var(--brd);
    }

    .fg {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
      gap: 15px;
      margin-bottom: 20px;
    }

    /* TABLE */
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 10px;
      overflow-x: auto;
    }

    thead {
      background: #151515;
    }

    th {
      text-align: left;
      font-size: 10px;
      color: var(--mut);
      padding: 12px 10px;
      text-transform: uppercase;
      letter-spacing: 1px;
      font-weight: 600;
    }

    td {
      padding: 12px 10px;
      border-bottom: 1px solid #222;
      font-size: 13px;
    }

    tr:hover {
      background: rgba(229, 9, 20, 0.05);
    }

    /* BADGES */
    .badge {
      padding: 5px 10px;
      border-radius: 4px;
      font-size: 10px;
      font-weight: bold;
      text-transform: uppercase;
      display: inline-block;
      letter-spacing: 0.5px;
    }

    .badge.ativo {
      background: rgba(0, 230, 118, 0.15);
      color: var(--grn);
    }

    .badge.vencido {
      background: rgba(229, 9, 20, 0.15);
      color: var(--red);
    }

    /* ANIMATIONS */
    @keyframes riseIn {
      from {
        opacity: 0;
        transform: translateY(20px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    /* RESPONSIVE */
    @media (max-width: 768px) {
      .login-wrap {
        width: 90%;
      }

      .modal-box {
        width: 90%;
      }

      .pix-section {
        flex-direction: column;
        text-align: center;
      }

      .dash-greet h1 {
        font-size: 2rem;
      }

      .fg {
        grid-template-columns: 1fr;
      }

      table {
        font-size: 12px;
      }

      th, td {
        padding: 8px 5px;
      }
    }

    /* UTILITIES */
    .success-msg {
      background: rgba(0, 230, 118, .1);
      border: 1px solid rgba(0, 230, 118, .25);
      padding: 11px;
      font-size: 0.83rem;
      color: var(--grn);
      margin-top: 14px;
      display: none;
      border-radius: 5px;
    }

    .success-msg.show {
      display: block;
      animation: slideDown 0.3s ease;
    }
  </style>
</head>
<body>

  <!-- LOGIN PAGE -->
  <div id="pg-login" class="page active">
    <div class="login-wrap">
      <div class="login-logo">Stream<span>Portal</span></div>
      <p class="login-tagline">Acesso exclusivo para assinantes</p>
      <div class="login-box">
        <div class="fld">
          <label for="li-id">ID do Cliente</label>
          <input type="text" id="li-id" placeholder="Ex: CLI-001">
        </div>
        <div class="fld">
          <label for="li-pw">Senha</label>
          <input type="password" id="li-pw" placeholder="••••••••">
        </div>
        <button class="btn-main" onclick="doLogin()">ENTRAR NO PORTAL</button>
        <div class="err-box" id="li-err">❌ Acesso negado. Verifique os dados.</div>
      </div>
      <div class="login-foot">
        <a href="#" onclick="goPage('pg-adm-login'); return false;">Acesso Administrativo →</a>
      </div>
    </div>
  </div>

  <!-- ADMIN LOGIN PAGE -->
  <div id="pg-adm-login" class="page">
    <div class="login-wrap">
      <div class="login-logo">Painel<span>Admin</span></div>
      <div class="login-box">
        <div class="fld">
          <label for="adm-u">Usuário</label>
          <input type="text" id="adm-u" placeholder="Usuário">
        </div>
        <div class="fld">
          <label for="adm-p">Senha</label>
          <input type="password" id="adm-p" placeholder="••••••••">
        </div>
        <button class="btn-main" onclick="doAdmLogin()">ACESSAR PAINEL</button>
        <div class="err-box" id="adm-err">❌ Credenciais inválidas.</div>
        <div class="login-foot">
          <a href="#" onclick="goPage('pg-login'); return false;">← Voltar</a>
        </div>
      </div>
    </div>
  </div>

  <!-- CLIENT DASHBOARD -->
  <div id="pg-dash" class="page">
    <div class="topbar">
      <div class="topbar-title">Stream<span>Portal</span></div>
      <button class="btn-main" style="width: auto; padding: 6px 16px; font-size: 0.85rem;" onclick="doLogout()">Sair</button>
    </div>
    <div class="dash-inner">
      <div class="dash-greet">
        <h1>OLÁ, <span id="d-name">...</span> 👋</h1>
      </div>
      <div class="kpi-row">
        <div class="kpi">
          <div class="kpi-lbl">Status</div>
          <div id="d-status">...</div>
        </div>
        <div class="kpi">
          <div class="kpi-lbl">Vencimento</div>
          <div class="kpi-val" id="d-venc">...</div>
        </div>
        <div class="kpi g">
          <div class="kpi-lbl">Valor Mensal</div>
          <div class="kpi-val" id="d-valor">...</div>
        </div>
      </div>
      <div class="pix-section">
        <div>
          <h3>PAGAR MENSALIDADE</h3>
          <p>Gere sua chave PIX para renovação imediata.</p>
        </div>
        <button class="btn-pix" onclick="openPix()">⚡ GERAR PIX</button>
      </div>
    </div>
  </div>

  <!-- ADMIN PANEL -->
  <div id="pg-admin" class="page">
    <div class="topbar">
      <div class="topbar-title" style="color: var(--blu);">Painel<span style="color: var(--blu);">Admin</span></div>
      <button class="btn-main" style="width: auto; padding: 6px 16px; font-size: 0.85rem;" onclick="doLogout()">Sair</button>
    </div>
    <div class="admin-inner">
      <div class="tabs">
        <button class="tab-btn on" onclick="openTab(event, 't1')">+ Novo Cliente</button>
        <button class="tab-btn" onclick="openTab(event, 't2')">📋 Lista de Clientes</button>
      </div>

      <!-- TAB 1: NEW CLIENT FORM -->
      <div id="t1" class="tab-pane on">
        <div class="adm-form">
          <div class="fg">
            <div class="fld">
              <label for="f-id">ID</label>
              <input type="text" id="f-id" placeholder="CLI-001">
            </div>
            <div class="fld">
              <label for="f-nome">Nome</label>
              <input type="text" id="f-nome" placeholder="Nome do Cliente">
            </div>
            <div class="fld">
              <label for="f-pw">Senha</label>
              <input type="text" id="f-pw" placeholder="Senha" value="123">
            </div>
            <div class="fld">
              <label for="f-venc">Vencimento</label>
              <input type="date" id="f-venc">
            </div>
            <div class="fld">
              <label for="f-val">Valor Mensal</label>
              <input type="text" id="f-val" placeholder="29,90" value="29,90">
            </div>
            <div class="fld">
              <label for="f-status">Status</label>
              <select id="f-status">
                <option value="ativo">✅ Ativo</option>
                <option value="vencido">⏰ Vencido</option>
              </select>
            </div>
          </div>
          <button class="btn-main" onclick="saveClient()">💾 CADASTRAR CLIENTE</button>
          <div class="success-msg" id="save-success">✅ Cliente cadastrado com sucesso!</div>
        </div>
      </div>

      <!-- TAB 2: CLIENT LIST -->
      <div id="t2" class="tab-pane">
        <div style="overflow-x: auto;">
          <table>
            <thead>
              <tr>
                <th>ID</th>
                <th>NOME</th>
                <th>VENCIMENTO</th>
                <th>VALOR</th>
                <th>STATUS</th>
                <th>AÇÕES</th>
              </tr>
            </thead>
            <tbody id="lista-corpo">
              <tr>
                <td colspan="6" style="text-align: center; color: var(--mut);">Nenhum cliente cadastrado</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>

  <!-- PIX MODAL -->
  <div id="m-pix" class="modal-bg">
    <div class="modal-box">
      <h2 style="color: var(--grn); font-family: 'Bebas Neue'">🔑 CHAVE PIX</h2>
      <div id="pix-key" style="background: #000; padding: 15px; margin: 20px 0; font-family: monospace; font-size: 0.85rem; border: 1px dashed var(--grn); border-radius: 5px; word-break: break-all;">
        chave-exemplo@pix.com
      </div>
      <button class="btn-main" onclick="closePix()">FECHAR</button>
    </div>
  </div>

  <script>
    // ===== DATABASE =====
    const DB_KEY = 'streamportal_db_v2';
    
    let db = JSON.parse(localStorage.getItem(DB_KEY)) || {
      clientes: [
        {
          id: 'CLI-001',
          nome: 'João Silva',
          senha: '123',
          venc: '2026-12-31',
          valor: '29,90',
          status: 'ativo'
        }
      ],
      config: {
        pix: 'seuemail@pix.com'
      }
    };

    // Save database to localStorage
    function saveDB() {
      localStorage.setItem(DB_KEY, JSON.stringify(db));
    }

    // ===== NAVIGATION =====
    function goPage(pageId) {
      // Hide all pages
      document.querySelectorAll('.page').forEach(page => {
        page.classList.remove('active');
      });

      // Show target page
      const targetPage = document.getElementById(pageId);
      if (targetPage) {
        targetPage.classList.add('active');
      } else {
        console.error(`Página #${pageId} não encontrada`);
      }
    }

    // ===== LOGIN & LOGOUT =====
    function doLogin() {
      const clientId = document.getElementById('li-id').value.trim();
      const password = document.getElementById('li-pw').value.trim();
      const errorBox = document.getElementById('li-err');

      // Clear previous error
      errorBox.classList.remove('show');

      // Validate inputs
      if (!clientId || !password) {
        errorBox.innerText = '❌ Preencha todos os campos.';
        errorBox.classList.add('show');
        return;
      }

      // Find user
      const user = db.clientes.find(u => u.id === clientId && u.senha === password);

      if (user) {
        // Populate dashboard
        document.getElementById('d-name').innerText = user.nome;
        document.getElementById('d-status').innerHTML = `<span class="badge ${user.status}">${user.status.toUpperCase()}</span>`;
        document.getElementById('d-venc').innerText = formatDate(user.venc);
        document.getElementById('d-valor').innerText = 'R$ ' + user.valor;

        // Store current user in session
        sessionStorage.setItem('currentUser', JSON.stringify(user));

        // Navigate to dashboard
        goPage('pg-dash');

        // Clear form
        document.getElementById('li-id').value = '';
        document.getElementById('li-pw').value = '';
      } else {
        errorBox.innerText = '❌ ID ou senha incorretos.';
        errorBox.classList.add('show');
      }
    }

    function doAdmLogin() {
      const username = document.getElementById('adm-u').value.trim();
      const password = documdossll
