<!DOCTYPE html>
<html lang="ur" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>میرا ماہانہ حساب</title>
    <!-- Chart.js CDN for Graphs -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --primary-color: #0d9488;
            --bg-color: #f1f5f9;
            --card-bg: #ffffff;
            --text-color: #1e293b;
            --danger-color: #e11d48;
            --success-color: #16a34a;
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
            padding: 15px;
        }

        header {
            text-align: center;
            padding: 15px 0;
            background: var(--primary-color);
            color: white;
            border-radius: 10px;
            margin-bottom: 20px;
            box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);
        }

        /* Nav Tabs */
        .nav-tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            justify-content: center;
            flex-wrap: wrap;
        }

        .tab-btn {
            background: #cbd5e1;
            border: none;
            padding: 12px 25px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            font-size: 1.1rem;
            transition: 0.3s;
        }

        .tab-btn.active {
            background: var(--primary-color);
            color: white;
        }

        /* Layout Container */
        .portal-container {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .section-card {
            background: var(--card-bg);
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            display: none; /* Hidden by default, controlled by JS tabs */
        }

        .section-card.active-section {
            display: block;
        }

        .section-title {
            border-bottom: 2px solid var(--primary-color);
            padding-bottom: 8px;
            margin-bottom: 15px;
            color: var(--primary-color);
        }

        /* Dashboard Stats */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
            gap: 12px;
            margin-bottom: 20px;
        }

        .stat-box {
            padding: 15px;
            border-radius: 8px;
            text-align: center;
            color: white;
        }

        .income-bg { background-color: var(--success-color); }
        .expense-bg { background-color: var(--danger-color); }
        .balance-bg { background-color: #0284c7; }

        /* Form Styling */
        .input-group {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-bottom: 20px;
        }

        input, select, button, textarea {
            padding: 10px;
            border: 1px solid #cbd5e1;
            border-radius: 6px;
            font-size: 1rem;
        }

        .btn-add {
            background: var(--primary-color);
            color: white;
            border: none;
            cursor: pointer;
            font-weight: bold;
        }

        /* Graphs Section Container */
        .charts-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 20px;
            margin-bottom: 20px;
        }

        .chart-box {
            background: #f8fafc;
            border: 1px solid #e2e8f0;
            padding: 15px;
            border-radius: 8px;
            text-align: center;
        }

        .chart-box h4 {
            margin-bottom: 10px;
            color: var(--text-color);
        }

        /* History List */
        .history-list {
            list-style: none;
            max-height: 200px;
            overflow-y: auto;
        }

        .history-item {
            display: flex;
            justify-content: space-between;
            padding: 8px 0;
            border-bottom: 1px solid #e2e8f0;
        }
    </style>
</head>
<body onload="initApp()">

    <header>
        <h1>میرا ماہانہ حساب</h1>
    </header>

    <!-- Navigation Tabs -->
    <div class="nav-tabs">
        <button class="tab-btn active" onclick="switchTab('tab-hisab', event)">۱. میرا حساب</button>
        <button class="tab-btn" onclick="switchTab('tab-form', event)">نیا اندراج کریں</button>
        <button class="tab-btn" onclick="switchTab('tab-plan', event)">۲. میرا ترقیاتی منصوبہ</button>
    </div>

    <div class="portal-container">

        <!-- SECTION 1: میرا حساب (Dashboard, Graphs & History) -->
        <div id="tab-hisab" class="section-card active-section">
            <h2 class="section-title">📊 میرا حساب (لائیو ڈیش بورڈ)</h2>

            <!-- Stats Bar -->
            <div class="stats-grid">
                <div class="stat-box income-bg">
                    <h4>کل آمدن</h4>
                    <p id="total-income">Rs. 0</p>
                </div>
                <div class="stat-box expense-bg">
                    <h4>کل خرچہ</h4>
                    <p id="total-expense">Rs. 0</p>
                </div>
                <div class="stat-box balance-bg">
                    <h4>بقايا رقم</h4>
                    <p id="net-balance">Rs. 0</p>
                </div>
            </div>

            <!-- 3 Financial Graphs Section -->
            <h3 style="margin-top: 15px;">📈 مالیاتی گراف (آمدن، خرچ، بچت):</h3>
            <div class="charts-container">
                <div class="chart-box">
                    <h4>آمدن کا گراف</h4>
                    <canvas id="incomeChart"></canvas>
                </div>
                <div class="chart-box">
                    <h4>خرچے کا گراف</h4>
                    <canvas id="expenseChart"></canvas>
                </div>
                <div class="chart-box">
                    <h4>بچت (بقايا) کا گراف</h4>
                    <canvas id="balanceChart"></canvas>
                </div>
            </div>

            <h3 style="margin-top: 20px;">حالیہ تاریخچہ (History):</h3>
            <ul id="history" class="history-list"></ul>
        </div>

        <!-- SECTION 2: نیا اندراج کرنے کا فارم (اب الگ ٹیب/صفحے پر ہے) -->
        <div id="tab-form" class="section-card">
            <h2 class="section-title">📝 نیا اندراج کریں</h2>
            <div class="input-group">
                <input type="text" id="desc" placeholder="تفصیل (مثلاً: گھر کا راشن، تنخواہ)">
                <input type="number" id="amount" placeholder="رقم (روپے)">
                <select id="type">
                    <option value="income">آمدن (+)</option>
                    <option value="expense">خرچہ (-)</option>
                </select>
                <select id="folder-select">
                    <option value="عام">عام فولڈر</option>
                    <option value="گروسری">گروسری و راشن</option>
                    <option value="یوٹیلٹی بلز">یوٹیلٹی بلز</option>
                    <option value="صحت و ادویات">صحت و ادویات</option>
                </select>
                <button class="btn-add" onclick="addTransaction()">شامل کریں</button>
            </div>
            <p id="form-status" style="color: var(--success-color); font-weight: bold; margin-top: 10px;"></p>
        </div>

        <!-- SECTION 3: میرا ترقیاتی منصوبہ -->
        <div id="tab-plan" class="section-card">
            <h2 class="section-title">🚀 میرا ترقیاتی منصوبہ</h2>
            <p style="margin-bottom: 15px;">اپنے مالی اور ذاتی ترقیاتی اہداف یہاں درج کریں:</p>
            <div class="input-group">
                <textarea id="dev-plan-text" rows="6" placeholder="اپنے اهداف اور منصوبے یہاں لکھیں..." style="resize: vertical;"></textarea>
                <button class="btn-add" onclick="saveDevPlan()">منصوبہ محفوظ کریں</button>
            </div>
            <p id="plan-status" style="color: var(--success-color); font-weight: bold; margin-top: 10px;"></p>
        </div>

    </div>

    <script>
        let totalIncome = 0;
        let totalExpense = 0;
        let historyData = [];
        
        let incomeChart, expenseChart, balanceChart;

        function initApp() {
            loadData();
            initCharts();
            updateDashboard();
        }

        // Initialize Chart.js instances
        function initCharts() {
            const commonOptions = {
                responsive: true,
                maintainAspectRatio: true,
                plugins: { legend: { display: false } }
            };

            incomeChart = new Chart(document.getElementById('incomeChart'), {
                type: 'bar',
                data: {
                    labels: ['آمدن'],
                    datasets: [{ data: [totalIncome], backgroundColor: '#16a34a' }]
                },
                options: commonOptions
            });

            expenseChart = new Chart(document.getElementById('expenseChart'), {
                type: 'bar',
                data: {
                    labels: ['خرچہ'],
                    datasets: [{ data: [totalExpense], backgroundColor: '#e11d48' }]
                },
                options: commonOptions
            });

            balanceChart = new Chart(document.getElementById('balanceChart'), {
                type: 'bar',
                data: {
                    labels: ['بچت/بقايا'],
                    datasets: [{ data: [totalIncome - totalExpense], backgroundColor: '#0284c7' }]
                },
                options: commonOptions
            });
        }

        function updateCharts() {
            incomeChart.data.datasets[0].data = [totalIncome];
            incomeChart.update();

            expenseChart.data.datasets[0].data = [totalExpense];
            expenseChart.update();

            balanceChart.data.datasets[0].data = [totalIncome - totalExpense];
            balanceChart.update();
        }

        function addTransaction() {
            const desc = document.getElementById('desc').value;
            const amount = parseFloat(document.getElementById('amount').value);
            const type = document.getElementById('type').value;
            const folder = document.getElementById('folder-select').value;

            if (!desc || isNaN(amount) || amount <= 0) {
                alert('براہ کرم صحیح تفصیل اور رقم درج کریں!');
                return;
            }

            if (type === 'income') {
                totalIncome += amount;
            } else {
                totalExpense += amount;
            }

            const transaction = { desc, amount, type, folder };
            historyData.unshift(transaction);

            updateDashboard();
            updateCharts();
            renderHistory();

            // Clear inputs
            document.getElementById('desc').value = '';
            document.getElementById('amount').value = '';
            
            saveData();
            
            document.getElementById('form-status').innerText = 'اندراج کامیابی سے شامل کر لیا گیا ہے!';
            setTimeout(() => {
                document.getElementById('form-status').innerText = '';
            }, 3000);
        }

        function updateDashboard() {
            document.getElementById('total-income').innerText = `Rs. ${totalIncome}`;
            document.getElementById('total-expense').innerText = `Rs. ${totalExpense}`;
            document.getElementById('net-balance').innerText = `Rs. ${totalIncome - totalExpense}`;
        }

        function renderHistory() {
            const historyList = document.getElementById('history');
            historyList.innerHTML = '';
            historyData.forEach(item => {
                const li = document.createElement('li');
                li.className = 'history-item';
                li.innerHTML = `<span>${item.desc} (${item.folder})</span> <strong style="color: ${item.type === 'income' ? 'green' : 'red'};">${item.type === 'income' ? '+' : '-'} Rs. ${item.amount}</strong>`;
                historyList.appendChild(li);
            });
        }

        // Tab Switching Logic
        function switchTab(tabId, event) {
            const sections = document.querySelectorAll('.section-card');
            sections.forEach(sec => sec.classList.remove('active-section'));

            document.getElementById(tabId).classList.add('active-section');

            const buttons = document.querySelectorAll('.tab-btn');
            buttons.forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
        }

        function saveData() {
            const devPlan = document.getElementById('dev-plan-text').value;
            const data = { totalIncome, totalExpense, historyData, devPlan };
            localStorage.setItem('my_finance_data', JSON.stringify(data));
        }

        function saveDevPlan() {
            saveData();
            document.getElementById('plan-status').innerText = 'منصوبہ کامیابی سے محفوظ ہو گیا ہے!';
            setTimeout(() => {
                document.getElementById('plan-status').innerText = '';
            }, 3000);
        }

        function loadData() {
            const saved = localStorage.getItem('my_finance_data');
            if (saved) {
                const data = JSON.parse(saved);
                totalIncome = data.totalIncome || 0;
                totalExpense = data.totalExpense || 0;
                historyData = data.historyData || [];
                if (data.devPlan) {
                    document.getElementById('dev-plan-text').value = data.devPlan;
                }
                renderHistory();
            }
        }
    </script>
</body>
</html>
