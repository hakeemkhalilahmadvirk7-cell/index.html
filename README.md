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
            display: flex;
            flex-direction: column;
            min-height: 100vh;
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

        /* Portal Container & Layout */
        .portal-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .section-card {
            background: var(--card-bg);
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            display: none; /* Hidden by default */
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

        /* Dashboard Stats Grid (Now includes Dev Plan button aligned with stats) */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
            gap: 12px;
            margin-bottom: 20px;
            align-items: stretch;
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

        .plan-btn-box {
            background-color: var(--primary-color);
            padding: 15px;
            border-radius: 8px;
            text-align: center;
            color: white;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            transition: 0.3s;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .plan-btn-box:hover {
            opacity: 0.9;
            transform: translateY(-2px);
        }

        .plan-btn-box h4 {
            font-size: 1.1rem;
        }

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

        /* Footer Styling */
        footer {
            margin-top: 30px;
            background: #1e293b;
            color: white;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
        }

        .footer-links {
            display: flex;
            justify-content: center;
            gap: 20px;
            flex-wrap: wrap;
            margin-bottom: 10px;
        }

        .footer-btn {
            background: transparent;
            border: 1px solid #cbd5e1;
            color: white;
            padding: 8px 15px;
            border-radius: 6px;
            cursor: pointer;
            transition: 0.3s;
            font-size: 0.95rem;
        }

        .footer-btn:hover {
            background: var(--primary-color);
            border-color: var(--primary-color);
        }
    </style>
</head>
<body onload="initApp()">

    <header>
        <h1>میرا ماہانہ حساب</h1>
    </header>

    <div class="portal-container">

        <!-- SECTION 1: مرکزی ڈیش بورڈ (کل آمدن، خرچ، بچت اور ترقیاتی منصوبے کا بٹن) -->
        <div id="tab-hisab" class="section-card active-section">
            <h2 class="section-title">📊 مرکزی مالیاتی ڈیش بورڈ</h2>

            <!-- Stats & Plan Button Grid -->
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
                <!-- Developer Plan Button aligned with stats -->
                <div class="plan-btn-box" onclick="switchSection('tab-plan')">
                    <h4>🚀 میرا ترقیاتی منصوبہ</h4>
                    <p style="font-size: 0.85rem; margin-top: 5px;">نیا پیج کھولیں</p>
                </div>
            </div>

            <!-- Transaction Form (ابھی ڈیش بورڈ میں شامل ہے تاکہ ڈیٹا درج کیا جا سکے) -->
            <h3 style="margin-top: 20px;">نیا اندراج کریں:</h3>
            <div class="input-group" style="margin-top: 10px;">
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

        <!-- SECTION 2: میرا ترقیاتی منصوبہ (نیا پیج) -->
        <div id="tab-plan" class="section-card">
            <button class="footer-btn" style="background: #64748b; margin-bottom: 15px;" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            <h2 class="section-title">🚀 میرا ترقیاتی منصوبہ</h2>
            <p style="margin-bottom: 15px;">اپنے مالی اور ذاتی ترقیاتی اہداف یہاں درج کریں:</p>
            <div class="input-group">
                <textarea id="dev-plan-text" rows="6" placeholder="اپنے اہداف اور منصوبے یہاں لکھیں..." style="resize: vertical;"></textarea>
                <button class="btn-add" onclick="saveDevPlan()">منصوبہ محفوظ کریں</button>
            </div>
            <p id="plan-status" style="color: var(--success-color); font-weight: bold; margin-top: 10px;"></p>
        </div>

        <!-- SECTION 3: About Us (نیا پیج) -->
        <div id="tab-about" class="section-card">
            <button class="footer-btn" style="background: #64748b; margin-bottom: 15px;" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            <h2 class="section-title">ℹ️ ہمارے بارے میں (About Us)</h2>
            <p style="line-height: 1.8; color: #475569;">
                "میرا ماہانہ حساب" ایک جدید اور آسان ڈیجیٹل مالیاتی پلیٹ فارم ہے جو آپ کو اپنے روزمرہ کے اخراجات، آمدن، اور بچت کا حساب کتاب رکھنے میں مدد فراہم کرتا ہے۔ اس کا مقصد آپ کی مالی زندگی کو منظم بنانا اور آپ کے ترقیاتی اہداف کو آسان بنانا ہے۔
            </p>
        </div>

        <!-- SECTION 4: Privacy Policy (نیا پیج) -->
        <div id="tab-privacy" class="section-card">
            <button class="footer-btn" style="background: #64748b; margin-bottom: 15px;" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            <h2 class="section-title">🔒 پرائیویسی پالیسی (Privacy Policy)</h2>
            <p style="line-height: 1.8; color: #475569;">
                آپ کی رازداری ہمارے لیے انتہائی اہم ہے۔ اس ایپلیکیشن میں درج کیا جانے والا تمام مالیاتی ڈیٹا اور معلومات آپ کے اپنے براؤزر کے لوکل سٹوریج (Local Storage) میں محفوظ ہوتی ہیں، اور کسی بھی بیرونی سرور پر منتقل نہیں کی جاتیں۔ آپ کا ڈیٹا مکمل طور پر محفوظ ہے۔
            </p>
        </div>

        <!-- SECTION 5: Contact Us (نیا پیج) -->
        <div id="tab-contact" class="section-card">
            <button class="footer-btn" style="background: #64748b; margin-bottom: 15px;" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            <h2 class="section-title">📞 رابطہ کریں (Contact Us)</h2>
            <p style="margin-bottom: 15px; color: #475569;">اگر آپ کو کوئی سوال پوچھنا ہو یا کوئی تجاویز دینی ہوں تو آپ ہم سے رابطہ کر سکتے ہیں:</p>
            <div style="background: #f8fafc; padding: 15px; border-radius: 8px; border: 1px solid #e2e8f0;">
                <p><strong>ای میل:</strong> support@merasahisab.com</p>
                <p style="margin-top: 8px;"><strong>فون:</strong> +92 300 0000000</p>
            </div>
        </div>

    </div>

    <!-- FOOTER SECTION -->
    <footer>
        <div class="footer-links">
            <button class="footer-btn" onclick="switchSection('tab-about')">About Us</button>
            <button class="footer-btn" onclick="switchSection('tab-privacy')">Privacy</button>
            <button class="footer-btn" onclick="switchSection('tab-contact')">Contact Us</button>
        </div>
        <p style="font-size: 0.85rem; color: #94a3b8; margin-top: 10px;">&copy; 2026 میرا ماہانہ حساب - جملہ حقوق محفوظ ہیں۔</p>
    </footer>

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

        // Section/Page Switching Logic
        function switchSection(sectionId) {
            const sections = document.querySelectorAll('.section-card');
            sections.forEach(sec => sec.classList.remove('active-section'));

            document.getElementById(sectionId).classList.add('active-section');
            window.scrollTo({ top: 0, behavior: 'smooth' });
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
