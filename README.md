<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Assistente de Gestão de Apostas</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .gradient-bg {
      background: linear-gradient(135deg, #1a2a6c 0%, #b21f1f 50%, #fdbb2d 100%);
    }
    .blink-warning {
      animation: blink 1s step-end infinite;
    }
    @keyframes blink {
      50% { opacity: 0.5; }
    }
    .stat-card {
      transition: all 0.3s ease;
    }
    .stat-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 10px 20px rgba(0,0,0,0.2);
    }
  </style>
</head>
<body class="bg-gray-100 font-sans">
  <div class="min-h-screen">
    <!-- Header -->
    <header class="gradient-bg text-white py-6 px-4 shadow-lg">
      <div class="container mx-auto flex flex-col md:flex-row justify-between items-center">
        <div class="flex items-center mb-4 md:mb-0">
          <img src="https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/a59cea51-b914-4aac-8e46-6ad731dc58cb.png" alt="Ícone de gráfico de crescimento com setas verdes em fundo preto simbolizando crescimento financeiro" class="mr-3 rounded-full">
          <h1 class="text-2xl md:text-3xl font-bold">Assistente de Apostas</h1>
        </div>
        <div class="bg-white text-gray-800 px-4 py-2 rounded-full shadow">
          <span class="font-medium">Saldo simulado: </span>
          <span id="balance" class="font-bold">R$ 100,00</span>
        </div>
      </div>
    </header>

    <!-- Warning Banner -->
    <div class="bg-red-600 text-white py-3 px-4 text-center blink-warning">
      <p class="font-medium">ATENÇÃO: Este é apenas um simulador educativo. Jogos de azar podem causar dependência. Jogue com responsabilidade.</p>
    </div>

    <main class="container mx-auto py-8 px-4">
      <!-- Dashboard -->
      <section class="mb-12">
        <h2 class="text-2xl font-bold mb-6 text-gray-800 border-b pb-2">Painel de Controle</h2>
        
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
          <!-- Stat Cards -->
          <div class="stat-card bg-white p-6 rounded-xl shadow-md">
            <h3 class="text-lg font-semibold text-gray-700 mb-2">Retorno Diário</h3>
            <p class="text-3xl font-bold text-green-600">+R$ <span id="daily-return">23,50</span></p>
            <p class="text-sm text-gray-500">Média dos últimos 7 dias</p>
          </div>
          
          <div class="stat-card bg-white p-6 rounded-xl shadow-md">
            <h3 class="text-lg font-semibold text-gray-700 mb-2">Apostas Hoje</h3>
            <p class="text-3xl font-bold text-blue-600"><span id="today-bets">8</span></p>
            <p class="text-sm text-gray-500">5 vitórias, 3 derrotas</p>
          </div>
          
          <div class="stat-card bg-white p-6 rounded-xl shadow-md">
            <h3 class="text-lg font-semibold text-gray-700 mb-2">Limite Diário</h3>
            <p class="text-3xl font-bold text-orange-600">R$ <span id="daily-limit">50,00</span></p>
            <p class="text-sm text-gray-500">R$ 35,00 utilizados</p>
          </div>
          
          <div class="stat-card bg-white p-6 rounded-xl shadow-md">
            <h3 class="text-lg font-semibold text-gray-700 mb-2">Taxa de Sucesso</h3>
            <p class="text-3xl font-bold text-purple-600"><span id="win-rate">62</span>%</p>
            <p class="text-sm text-gray-500">Semana atual</p>
          </div>
        </div>
      </section>

      <!-- Simulator Section -->
      <section class="mb-12">
        <h2 class="text-2xl font-bold mb-6 text-gray-800 border-b pb-2">Simulador de Probabilidades</h2>
        
        <div class="bg-white p-6 rounded-xl shadow-md">
          <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
            <div>
              <h3 class="text-lg font-semibold mb-4">Calculadora de Retorno</h3>
              <div class="space-y-4">
                <div>
                  <label class="block text-gray-700 mb-2">Valor da Aposta (R$)</label>
                  <input type="number" id="bet-amount" class="w-full p-2 border rounded" value="10" min="1">
                </div>
                <div>
                  <label class="block text-gray-700 mb-2">Probabilidade (%)</label>
                  <input type="range" id="probability" class="w-full" min="1" max="100" value="50">
                  <p class="text-right"><span id="probability-value">50</span>%</p>
                </div>
                <div>
                  <label class="block text-gray-700 mb-2">Odd (Multiplicador)</label>
                  <input type="number" step="0.1" id="odd" class="w-full p-2 border rounded" value="2.0">
                </div>
                <button id="calculate-btn" class="bg-blue-600 text-white py-2 px-6 rounded hover:bg-blue-700 transition">
                  Calcular Retorno Esperado
                </button>
              </div>
            </div>
            
            <div>
              <h3 class="text-lg font-semibold mb-4">Resultados Esperados</h3>
              <div class="bg-gray-50 p-4 rounded-lg">
                <div class="mb-4">
                  <p class="text-gray-700">Retorno esperado em 100 apostas:</p>
                  <p class="text-xl font-bold text-green-600 mb-2">+R$ <span id="expected-return">0,00</span></p>
                  <p class="text-sm text-gray-500">Valor esperado por aposta: R$ <span id="expected-per-bet">0,00</span></p>
                </div>
                
                <div>
                  <p class="text-gray-700">Chance de ter prejuízo:</p>
                  <div class="h-4 bg-gray-200 rounded-full mt-2 overflow-hidden">
                    <div id="loss-risk-bar" class="h-full bg-red-500" style="width: 50%"></div>
                  </div>
                  <p class="text-right text-sm text-gray-700 mt-1">
                    <span id="loss-risk-value">50</span>% de chance de perder dinheiro
                  </p>
                </div>
              </div>
              
              <div class="mt-6 bg-yellow-50 border-l-4 border-yellow-400 p-4">
                <p class="text-yellow-700">
                  <strong>Nota:</strong> Estas são probabilidades teóricas baseadas em matemática. Resultados reais podem variar significativamente no curto prazo.
                </p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Bet History -->
      <section class="mb-12">
        <div class="flex justify-between items-center mb-6">
          <h2 class="text-2xl font-bold text-gray-800">Histórico Simulado</h2>
          <div class="flex space-x-2">
            <button id="simulate-win" class="bg-green-100 text-green-800 py-1 px-3 rounded-full text-sm font-medium">
              Simular Vitória
            </button>
            <button id="simulate-loss" class="bg-red-100 text-red-800 py-1 px-3 rounded-full text-sm font-medium">
              Simular Derrota
            </button>
            <button id="reset-history" class="bg-gray-200 text-gray-800 py-1 px-3 rounded-full text-sm font-medium">
              Reiniciar
            </button>
          </div>
        </div>
        
        <div class="bg-white rounded-xl shadow-md overflow-hidden">
          <div class="overflow-x-auto">
            <table class="min-w-full divide-y divide-gray-200">
              <thead class="bg-gray-50">
                <tr>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Data</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Aposta</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Odd</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Resultado</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Retorno</th>
                </tr>
              </thead>
              <tbody id="history-body" class="bg-white divide-y divide-gray-200">
                <!-- JavaScript will populate this -->
              </tbody>
            </table>
          </div>
        </div>
      </section>

      <!-- Responsible Gaming -->
      <section class="bg-white p-6 rounded-xl shadow-md mb-8">
        <h2 class="text-2xl font-bold mb-4 text-gray-800">Jogo Consciente</h2>
        
        <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
          <div>
            <h3 class="text-lg font-semibold mb-3">Dicas de Gestão de Banca</h3>
            <ul class="space-y-3">
              <li class="flex items-start">
                <img src="https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/4fb4da5c-ead4-465c-9c05-30e71949d292.png" alt="Ícone de porcentagem representando gestão de risco" class="mr-2">
                <span>Nunca aposte mais de 1-2% da sua banca em uma única aposta</span>
              </li>
              <li class="flex items-start">
                <img src="https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/4a5e10c5-e4c3-4c9a-a940-04d847e3becd.png" alt="Ícone de calendário representando controle diário" class="mr-2">
                <span>Defina um limite diário e nunca o ultrapasse</span>
              </li>
              <li class="flex items-start">
                <img src="https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/82ea8880-e914-4cd1-af44-9fe34e9b2bfa.png" alt="Ícone de gráfico de queda representando stop loss" class="mr-2">
                <span>Estabeleça um stop loss (ponto para parar após perdas)</span>
              </li>
              <li class="flex items-start">
                <img src="https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/6ae27399-e3be-4620-b361-c7883f20900e.png" alt="Ícone de cérebro representando controle emocional" class="mr-2">
                <span>Evite apostar por emoção ou para recuperar perdas</span>
              </li>
            </ul>
          </div>
          
          <div class="bg-red-50 border-l-4 border-red-400 p-4 rounded-r">
            <h3 class="text-lg font-semibold mb-3 text-red-800">Sinais de Alerta</h3>
            <p class="mb-3 text-red-700">Reconheça os sinais de que o jogo pode estar se tornando um problema:</p>
            <ul class="list-disc pl-5 space-y-2 text-red-700">
              <li>Pensar constantemente em apostar</li>
              <li>Apostar quantias cada vez maiores</li>
              <li>Tentar recuperar perdas com mais apostas</li>
              <li>Mentir sobre o tempo ou dinheiro gasto</li>
              <li>Negligenciar responsabilidades</li>
            </ul>
            <p class="mt-4 font-medium text-red-800">Se precisar de ajuda:</p>
            <p class="text-blue-600">Disque 188 - Centro de Valorização da Vida</p>
          </div>
        </div>
      </section>

      <!-- Bankroll Management -->
      <section class="bg-white p-6 rounded-xl shadow-md">
        <h2 class="text-2xl font-bold mb-6 text-gray-800">Gerenciamento de Banca</h2>
        
        <div class="space-y-6">
          <div>
            <div class="flex justify-between mb-2">
              <h3 class="font-medium">Limite Diário</h3>
              <span class="text-gray-600">R$ <span id="display-daily-limit">50</span></span>
            </div>
            <input type="range" id="daily-limit-slider" class="w-full" min="10" max="500" step="10" value="50">
          </div>
          
          <div>
            <div class="flex justify-between mb-2">
              <h3 class="font-medium">% por Aposta</h3>
              <span class="text-gray-600"><span id="display-per-bet">2</span>% da banca</span>
            </div>
            <input type="range" id="per-bet-slider" class="w-full" min="1" max="10" value="2">
          </div>
          
          <div class="bg-green-50 border-l-4 border-green-400 p-4 rounded-r">
            <h3 class="font-semibold mb-2">Simulador de Crescimento</h3>
            <p class="mb-3 text-sm">
              Projeção de crescimento com taxas atuais em 30 dias:<br>
              <span class="font-bold text-lg" id="growth-projection">R$ 1.342,50</span>
            </p>
            <p class="text-xs text-gray-600">
              *Baseado em 20 apostas por dia com taxa de sucesso atual de <span id="current-win-rate">62</span>%
            </p>
          </div>
        </div>
      </section>
    </main>

    <footer class="gradient-bg text-white py-6 px-4">
      <div class="container mx-auto text-center">
        <p class="mb-2">Este é um simulador educacional apenas. Não é uma garantia de resultados.</p>
        <p class="text-sm opacity-75">Jogue com responsabilidade. O jogo pode ser prejudicial se não for controlado.</p>
      </div>
    </footer>
  </div>

  <script>
    // Initial Data
    let balance = 100.00;
    let history = [
      { date: '2023-11-01', amount: 5.00, odd: 1.8, result: 'win', return: 9.00 },
      { date: '2023-11-01', amount: 10.00, odd: 2.5, result: 'loss', return: -10.00 },
      { date: '2023-11-02', amount: 7.50, odd: 3.0, result: 'win', return: 22.50 },
      { date: '2023-11-02', amount: 5.00, odd: 1.5, result: 'win', return: 7.50 }
    ];
    
    // DOM Elements
    const balanceEl = document.getElementById('balance');
    const dailyReturnEl = document.getElementById('daily-return');
    const todayBetsEl = document.getElementById('today-bets');
    const dailyLimitEl = document.getElementById('daily-limit');
    const winRateEl = document.getElementById('win-rate');
    const probabilityInput = document.getElementById('probability');
    const probabilityValue = document.getElementById('probability-value');
    const betAmountInput = document.getElementById('bet-amount');
    const oddInput = document.getElementById('odd');
    const expectedReturnEl = document.getElementById('expected-return');
    const expectedPerBetEl = document.getElementById('expected-per-bet');
    const lossRiskBar = document.getElementById('loss-risk-bar');
    const lossRiskValue = document.getElementById('loss-risk-value');
    const historyBody = document.getElementById('history-body');
    const simulateWinBtn = document.getElementById('simulate-win');
    const simulateLossBtn = document.getElementById('simulate-loss');
    const resetHistoryBtn = document.getElementById('reset-history');
    const dailyLimitSlider = document.getElementById('daily-limit-slider');
    const displayDailyLimit = document.getElementById('display-daily-limit');
    const perBetSlider = document.getElementById('per-bet-slider');
    const displayPerBet = document.getElementById('display-per-bet');
    const growthProjection = document.getElementById('growth-projection');
    const currentWinRate = document.getElementById('current-win-rate');
    
    // Initialize
    updateStats();
    renderHistory();
    updateCalculator();
    
    // Event Listeners
    probabilityInput.addEventListener('input', () => {
      probabilityValue.textContent = probabilityInput.value;
      updateCalculator();
    });
    
    betAmountInput.addEventListener('input', updateCalculator);
    oddInput.addEventListener('input', updateCalculator);
    
    document.getElementById('calculate-btn').addEventListener('click', () => {
      alert('Cálculos atualizados. Veja os resultados esperados ao lado.');
    });
    
    simulateWinBtn.addEventListener('click', () => {
      const amount = parseFloat((Math.random() * 20).toFixed(2));
      const odd = (Math.random() * 3 + 1).toFixed(1);
      const betReturn = parseFloat((amount * odd).toFixed(2));
      
      history.unshift({
        date: new Date().toISOString().split('T')[0],
        amount: amount,
        odd: odd,
        result: 'win',
        return: betReturn
      });
      
      balance += betReturn - amount;
      updateStats();
      renderHistory();
    });
    
    simulateLossBtn.addEventListener('click', () => {
      const amount = parseFloat((Math.random() * 20).toFixed(2));
      
      history.unshift({
        date: new Date().toISOString().split('T')[0],
        amount: amount,
        odd: (Math.random() * 3 + 1).toFixed(1),
        result: 'loss',
        return: -amount
      });
      
      balance -= amount;
      updateStats();
      renderHistory();
    });
    
    resetHistoryBtn.addEventListener('click', () => {
      if (confirm('Tem certeza que deseja reiniciar o histórico? Todos os dados serão perdidos.')) {
        history = [
          { date: '2023-11-01', amount: 5.00, odd: 1.8, result: 'win', return: 9.00 },
          { date: '2023-11-01', amount: 10.00, odd: 2.5, result: 'loss', return: -10.00 },
          { date: '2023-11-02', amount: 7.50, odd: 3.0, result: 'win', return: 22.50 },
          { date: '2023-11-02', amount: 5.00, odd: 1.5, result: 'win', return: 7.50 }
        ];
        balance = 100.00;
        updateStats();
        renderHistory();
      }
    });
    
    dailyLimitSlider.addEventListener('input', () => {
      displayDailyLimit.textContent = dailyLimitSlider.value;
      dailyLimitEl.textContent = dailyLimitSlider.value + ',00';
    });
    
    perBetSlider.addEventListener('input', () => {
      displayPerBet.textContent = perBetSlider.value;
    });
    
    // Functions
    function updateStats() {
      balanceEl.textContent = balance.toFixed(2).replace('.', ',');
      
      const today = new Date().toISOString().split('T')[0];
      const todayBets = history.filter(bet => bet.date === today).length;
      todayBetsEl.textContent = todayBets;
      
      const wins = history.filter(bet => bet.result === 'win').length;
      const total = history.length;
      const winRate = total > 0 ? Math.round((wins / total) * 100) : 0;
      winRateEl.textContent = winRate;
      currentWinRate.textContent = winRate;
      
      // Update projection
      const projectedGrowth = calculateProjection(balance, winRate/100, 30);
      growthProjection.textContent = 'R$ ' + projectedGrowth.toFixed(2).replace('.', ',');
    }
    
    function renderHistory() {
      historyBody.innerHTML = '';
      
      history.forEach(bet => {
        const row = document.createElement('tr');
        
        const dateCell = document.createElement('td');
        dateCell.className = 'px-6 py-4 whitespace-nowrap text-sm text-gray-500';
        dateCell.textContent = bet.date;
        
        const amountCell = document.createElement('td');
        amountCell.className = 'px-6 py-4 whitespace-nowrap text-sm text-gray-500';
        amountCell.textContent = 'R$ ' + bet.amount.toFixed(2).replace('.', ',');
        
        const oddCell = document.createElement('td');
        oddCell.className = 'px-6 py-4 whitespace-nowrap text-sm text-gray-500';
        oddCell.textContent = bet.odd;
        
        const resultCell = document.createElement('td');
        resultCell.className = 'px-6 py-4 whitespace-nowrap text-sm font-medium';
        resultCell.textContent = bet.result === 'win' ? 'Vitória' : 'Derrota';
        resultCell.classList.add(bet.result === 'win' ? 'text-green-600' : 'text-red-600');
        
        const returnCell = document.createElement('td');
        returnCell.className = 'px-6 py-4 whitespace-nowrap text-sm font-medium';
        returnCell.textContent = bet.return >= 0 ? '+' : '';
        returnCell.textContent += 'R$ ' + Math.abs(bet.return).toFixed(2).replace('.', ',');
        returnCell.classList.add(bet.return >= 0 ? 'text-green-600' : 'text-red-600');
        
        row.appendChild(dateCell);
        row.appendChild(amountCell);
        row.appendChild(oddCell);
        row.appendChild(resultCell);
        row.appendChild(returnCell);
