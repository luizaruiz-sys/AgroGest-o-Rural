<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AgroGestão - Sistema de Gestão Rural</title>
  <style>
    :root {
      --primary-color: #2e7d32;
      --primary-hover: #1b5e20;
      --bg-color: #f4f6f8;
      --card-bg: #ffffff;
      --text-color: #333333;
      --border-color: #e0e0e0;
      --danger-color: #c62828;
      --success-color: #2e7d32;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    body {
      background-color: var(--bg-color);
      color: var(--text-color);
      display: flex;
      flex-direction: column;
      min-height: 100vh;
    }

    header {
      background-color: var(--primary-color);
      color: white;
      padding: 1.2rem 2rem;
      text-align: center;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }

    nav {
      background-color: #ffffff;
      display: flex;
      justify-content: center;
      border-bottom: 1px solid var(--border-color);
      flex-wrap: wrap;
    }

    nav button {
      background: none;
      border: none;
      padding: 1rem 1.5rem;
      font-size: 1rem;
      font-weight: 600;
      color: #666;
      cursor: pointer;
      transition: all 0.3s ease;
      border-bottom: 3px solid transparent;
    }

    nav button:hover, nav button.active {
      color: var(--primary-color);
      border-bottom-color: var(--primary-color);
    }

    main {
      max-width: 1000px;
      width: 100%;
      margin: 2rem auto;
      padding: 0 1rem;
      flex: 1;
    }

    .tab-content {
      display: none;
      background: var(--card-bg);
      padding: 2rem;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.05);
    }

    .tab-content.active {
      display: block;
    }

    h2 {
      margin-bottom: 1.5rem;
      color: var(--primary-color);
    }

    .form-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 1rem;
      margin-bottom: 1.5rem;
    }

    .form-group {
      display: flex;
      flex-direction: column;
    }

    .form-group label {
      margin-bottom: 0.5rem;
      font-size: 0.9rem;
      font-weight: 600;
    }

    .form-group input, .form-group select {
      padding: 0.6rem;
      border: 1px solid var(--border-color);
      border-radius: 4px;
      font-size: 1rem;
    }

    .btn {
      background-color: var(--primary-color);
      color: white;
      border: none;
      padding: 0.75rem 1.5rem;
      font-size: 1rem;
      border-radius: 4px;
      cursor: pointer;
      transition: background 0.2s ease;
    }

    .btn:hover {
      background-color: var(--primary-hover);
    }

    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 1.5rem;
    }

    th, td {
      padding: 0.75rem;
      text-align: left;
      border-bottom: 1px solid var(--border-color);
    }

    th {
      background-color: #f8f9fa;
      font-weight: 600;
    }

    .badge-receita {
      color: var(--success-color);
      font-weight: bold;
    }

    .badge-despesa {
      color: var(--danger-color);
      font-weight: bold;
    }

    footer {
      text-align: center;
      padding: 1rem;
      background-color: #ffffff;
      border-top: 1px solid var(--border-color);
      font-size: 0.85rem;
      color: #777;
    }
  </style>
</head>
<body>

  <header>
    <h1>🌾 AgroGestão Rural</h1>
  </header>

  <nav>
    <button class="tab-link active" onclick="openTab(event, 'propriedades')">Propriedades</button>
    <button class="tab-link" onclick="openTab(event, 'producao')">Produção</button>
    <button class="tab-link" onclick="openTab(event, 'estoque')">Estoque de Insumos</button>
    <button class="tab-link" onclick="openTab(event, 'financeiro')">Financeiro</button>
  </nav>

  <main>
    <section id="propriedades" class="tab-content active">
      <h2>Cadastro de Propriedades</h2>
      <form id="form-propriedade" onsubmit="addPropriedade(event)">
        <div class="form-grid">
          <div class="form-group">
            <label for="prop-nome">Nome da Propriedade</label>
            <input type="text" id="prop-nome" required placeholder="Ex: Fazenda Boa Vista">
          </div>
          <div class="form-group">
            <label for="prop-area">Área Total (Hectares)</label>
            <input type="number" id="prop-area" step="0.1" required placeholder="Ex: 150">
          </div>
          <div class="form-group">
            <label for="prop-local">Localização/Município</label>
            <input type="text" id="prop-local" required placeholder="Ex: Ribeirão Preto - SP">
          </div>
        </div>
        <button type="submit" class="btn">Cadastrar Propriedade</button>
      </form>

      <table>
        <thead>
          <tr>
            <th>Nome</th>
            <th>Área (ha)</th>
            <th>Localização</th>
          </tr>
        </thead>
        <tbody id="table-propriedades"></tbody>
      </table>
    </section>

    <section id="producao" class="tab-content">
      <h2>Controle de Produção</h2>
      <form id="form-producao" onsubmit="addProducao(event)">
        <div class="form-grid">
          <div class="form-group">
            <label for="prod-cultura">Cultura/Atividade</label>
            <input type="text" id="prod-cultura" required placeholder="Ex: Milho, Soja, Leite">
          </div>
          <div class="form-group">
            <label for="prod-qtd">Quantidade Colhida/Produzida</label>
            <input type="number" id="prod-qtd" step="0.01" required placeholder="Ex: 500">
          </div>
          <div class="form-group">
            <label for="prod-unidade">Unidade</label>
            <select id="prod-unidade">
              <option value="Sacos">Sacos</option>
              <option value="Toneladas">Toneladas</option>
              <option value="Litros">Litros</option>
              <option value="Kg">Kg</option>
            </select>
          </div>
          <div class="form-group">
            <label for="prod-data">Data</label>
            <input type="date" id="prod-data" required>
          </div>
        </div>
        <button type="submit" class="btn">Registrar Produção</button>
      </form>

      <table>
        <thead>
          <tr>
            <th>Cultura</th>
            <th>Quantidade</th>
            <th>Data</th>
          </tr>
        </thead>
        <tbody id="table-producao"></tbody>
      </table>
    </section>

    <section id="estoque" class="tab-content">
      <h2>Estoque de Insumos</h2>
      <form id="form-estoque" onsubmit="addEstoque(event)">
        <div class="form-grid">
          <div class="form-group">
            <label for="est-item">Item / Insumo</label>
            <input type="text" id="est-item" required placeholder="Ex: Adubo NPK 20-05-20">
          </div>
          <div class="form-group">
            <label for="est-categoria">Categoria</label>
            <select id="est-categoria">
              <option value="Fertilizante">Fertilizante</option>
              <option value="Semente">Semente</option>
              <option value="Defensivo">Defensivo</option>
              <option value="Ração">Ração</option>
              <option value="Outro">Outro</option>
            </select>
          </div>
          <div class="form-group">
            <label for="est-qtd">Quantidade em Estoque</label>
            <input type="number" id="est-qtd" step="0.01" required placeholder="Ex: 50">
          </div>
        </div>
        <button type="submit" class="btn">Adicionar ao Estoque</button>
      </form>

      <table>
        <thead>
          <tr>
            <th>Item</th>
            <th>Categoria</th>
            <th>Quantidade</th>
          </tr>
        </thead>
        <tbody id="table-estoque"></tbody>
      </table>
    </section>

    <section id="financeiro" class="tab-content">
      <h2>Acompanhamento Financeiro</h2>
      <form id="form-financeiro" onsubmit="addFinanceiro(event)">
        <div class="form-grid">
          <div class="form-group">
            <label for="fin-desc">Descrição</label>
            <input type="text" id="fin-desc" required placeholder="Ex: Venda de lote de milho">
          </div>
          <div class="form-group">
            <label for="fin-tipo">Tipo de Operação</label>
            <select id="fin-tipo">
              <option value="Receita">Receita (+)</option>
              <option value="Despesa">Despesa (-)</option>
            </select>
          </div>
          <div class="form-group">
            <label for="fin-valor">Valor (R$)</label>
            <input type="number" id="fin-valor" step="0.01" required placeholder="Ex: 12500.00">
          </div>
        </div>
        <button type="submit" class="btn">Lançar Transação</button>
      </form>

      <table>
        <thead>
          <tr>
            <th>Descrição</th>
            <th>Tipo</th>
            <th>Valor (R$)</th>
          </tr>
        </thead>
        <tbody id="table-financeiro"></tbody>
      </table>
    </section>
  </main>

  <footer>
    <p>AgroGestão &copy; 2026 - Sistema de Controle Agrícola Simplificado</p>
  </footer>

  <script>
    // Gerenciador de Abas
    function openTab(evt, tabName) {
      const tabContents = document.getElementsByClassName("tab-content");
      for (let i = 0; i < tabContents.length; i++) {
        tabContents[i].classList.remove("active");
      }

      const tabLinks = document.getElementsByClassName("tab-link");
      for (let i = 0; i < tabLinks.length; i++) {
        tabLinks[i].classList.remove("active");
      }

      document.getElementById(tabName).classList.add("active");
      evt.currentTarget.classList.add("active");
    }

    // Funções de Adição e Manipulação de Dados
    function addPropriedade(e) {
      e.preventDefault();
      const nome = document.getElementById('prop-nome').value;
      const area = document.getElementById('prop-area').value;
      const local = document.getElementById('prop-local').value;

      const tbody = document.getElementById('table-propriedades');
      const row = `<tr><td>${nome}</td><td>${area} ha</td><td>${local}</td></tr>`;
      tbody.innerHTML += row;

      document.getElementById('form-propriedade').reset();
    }

    function addProducao(e) {
      e.preventDefault();
      const cultura = document.getElementById('prod-cultura').value;
      const qtd = document.getElementById('prod-qtd').value;
      const unidade = document.getElementById('prod-unidade').value;
      const data = document.getElementById('prod-data').value;

      const tbody = document.getElementById('table-producao');
      const row = `<tr><td>${cultura}</td><td>${qtd} ${unidade}</td><td>${data}</td></tr>`;
      tbody.innerHTML += row;

      document.getElementById('form-producao').reset();
    }

    function addEstoque(e) {
      e.preventDefault();
      const item = document.getElementById('est-item').value;
      const categoria = document.getElementById('est-categoria').value;
      const qtd = document.getElementById('est-qtd').value;

      const tbody = document.getElementById('table-estoque');
      const row = `<tr><td>${item}</td><td>${categoria}</td><td>${qtd}</td></tr>`;
      tbody.innerHTML += row;

      document.getElementById('form-estoque').reset();
    }

    function addFinanceiro(e) {
      e.preventDefault();
      const desc = document.getElementById('fin-desc').value;
      const tipo = document.getElementById('fin-tipo').value;
      const valor = parseFloat(document.getElementById('fin-valor').value).toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });

      const badgeClass = tipo === 'Receita' ? 'badge-receita' : 'badge-despesa';

      const tbody = document.getElementById('table-financeiro');
      const row = `<tr><td>${desc}</td><td class="${badgeClass}">${tipo}</td><td>${valor}</td></tr>`;
      tbody.innerHTML += row;

      document.getElementById('form-financeiro').reset();
    }
  </script>
</body>
</html>
