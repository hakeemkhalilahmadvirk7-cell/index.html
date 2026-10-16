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
            padding: 20px;
            display: flex;
            flex-direction: column;
            min-height: 100vh;
        }

        /* 1. بڑا ہیڈر */
        header {
            text-align: center;
            padding: 30px 15px;
            background: var(--primary-color);
            color: white;
            border-radius: 12px;
            margin-bottom: 25px;
            box-shadow: 0 6px 12px rgba(0,0,0,0.15);
        }

        header h1 {
            font-size: 2.8rem;
            font-weight: 800;
            letter-spacing: 1px;
        }

        /* Portal Container */
        .portal-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .section-card {
            background: var(--card-bg);
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            display: none; /* Hidden by default */
        }

        .section-card.active-section {
            display: block;
        }

        .section-title {
            border-bottom: 2px solid var(--primary-color);
            padding-bottom: 10px;
            margin-bottom: 20px;
            color: var(--primary-color);
            font-size: 1.6rem;
        }

        /* 2. چاروں بٹن ایک ہی لائن میں */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
            margin-bottom: 25px;
        }

        @media (max-width: 768px) {
            .stats-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }

        .stat-box-btn {
            padding: 10px 8px;
            border-radius: 8px;
            text-align: center;
            color: white;
            cursor: pointer;
            transition: transform 0.2s, opacity 0.2s;
            border: none;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .stat-box-btn:hover {
            opacity: 0.9;
            transform: translateY(-2px);
        }

        .stat-box-btn h4 {
            font-size: 0.95rem;
            font-weight: 600;
            margin-bottom: 4px;
        }

        .stat-box-btn p {
            font-size: 1.05rem;
            font-weight: bold;
        }

        .income-bg { background-color: var(--success-color); }
        .expense-bg { background-color: var(--danger-color); }
        .balance-bg { background-color: #0284c7; }
        .plan-bg { background-color: var(--primary-color); }

        /* Form Styling */
        .input-group {
            display: flex;
            flex-direction: column;
            gap: 12px;
            margin-bottom: 20px;
            max-width: 600px;
        }

        input, select, button, textarea {
            padding: 12px;
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

        .btn-back {
            background: #64748b;
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 6px;
            cursor: pointer;
            margin-bottom: 15px;
            font-size: 0.95rem;
        }

        /* 4. مالیاتی گراف ایک ہی لائن میں */
        .charts-container {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin-top: 15px;
        }

        @media (max-width: 900px) {
            .charts-container {
                grid-template-columns: 1fr;
            }
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
            font-size: 1rem;
        }

        /* 6. بڑا فوٹر */
        footer {
            margin-top: 40px;
            background: #0f172a;
            color: white;
            padding: 35px 20px;
            border-radius: 12px;
            text-align: center;
            box-shadow: 0 -4px 10px rgba(0,0,0,0.05);
        }

        .footer-links {
            display: flex;
            justify-content: center;
            gap: 25px;
            flex-wrap: wrap;
            margin-bottom: 15px;
        }

        .footer-btn {
            background: transparent;
            border: 1px solid #475569;
            color: white;
            padding: 10px 22px;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.3s;
            font-size: 1.05rem;
            font-weight: 500;
        }

        .footer-btn:hover {
            background: var(--primary-color);
            border-color: var(--primary-color);
        }
    </style>
</head>
<body onload="initApp()">

    <!-- 1. بڑا ہیڈر -->
    <header>
        <h1>میرا ماہانہ حساب</h1>
    </header>

    <div class="portal-container">

        <!-- 2. مین ڈیش بورڈ: چاروں بٹنز ایک لائن میں اور ایک ہی لائن میں 3 گراف -->
        <div id="tab-hisab" class="section-card active-section">
            <h2 class="section-title">📊 مرکزی مالیاتی ڈیش بورڈ</h2>

            <!-- چاروں بٹنز ایک ہی لائن میں -->
            <div class="stats-grid">
                <div class="stat-box-btn income-bg" onclick="switchSection('tab-income-page')">
                    <h4>کل آمدن</h4>
                    <p id="total-income">Rs. 0</p>
                </div>
                <div class="stat-box-btn expense-bg" onclick="switchSection('tab-expense-page')">
                    <h4>کل خرچہ</h4>
                    <p id="total-expense">Rs. 0</p>
                </div>
                <div class="stat-box-btn balance-bg" onclick="switchSection('tab-balance-page')">
                    <h4>بقايا رقم</h4>
                    <p id="net-balance">Rs. 0</p>
                </div>
                <div class="stat-box-btn plan-bg" onclick="switchSection('tab-plan')">
                    <h4>میرا ترقیاتی منصوبہ</h4>
                    <p style="font-size: 0.8rem; font-weight: normal;">🚀 کھولیں</p>
                </div>
            </div>

            <!-- 4. تینوں مالیاتی گراف ایک ہی لائن میں -->
            <h3 style="margin-top: 10px; margin-bottom: 10px;">📈 مالیاتی گراف (آمدن، خرچ، بچت):</h3>
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
            <!-- 5. حالیہ تاریخچہ (History) یہاں سے ختم کر دیا گیا ہے -->
        </div>

        <!-- 3. کل آمدن کا نیا صفحہ (جس میں نیا اندراج کا فارم موجود ہے) -->
        <div id="tab-income-page" class="section-card">
            <button class="btn-back" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            <h2 class="section-title">💵 کل آمدن اور نیا اندراج</h2>
            <div style="background: #f0fdf4; border: 1px solid #bbf7d0; padding: 15px; border-radius: 8px; margin-bottom: 20px;">
                <h3>موجودہ کل آمدن: <span id="income-page-total" style="color: var(--success-color);">Rs. 0</span></h3>
            </div>

            <!-- نیا اندراج کا فارم -->
            <h3>نیا اندراج کریں:</h3>
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
            <p id="form-status" style="color: var(--success-color); font-weight: bold; margin-top: 10px;"></p>
        </div>

        <!-- کل خرچ کا الگ صفحہ -->
        <div id="tab-expense-page" class="section-card">
            <button class="btn-back" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            <h2 class="section-title">💸 کل خرچ کی تفصیل</h2>
            <div style="background: #fff1f2; border: 1px solid #fecdd3; padding: 20px; border-radius: 8px;">
                <h3>موجودہ کل خرچہ: <span id="expense-page-total" style="color: var(--danger-color);">Rs. 0</span></h3>
            </div>
        </div>

        <!-- بقایا رقم کا الگ صفحہ -->
        <div id="tab-balance-page" class="section-card">
            <button class="btn-back" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            <h2 class="section-title">🏦 بقایا رقم (بچت)</h2>
            <div style="background: #f0f9ff; border: 1px solid #bae6fd; padding: 20px; border-radius: 8px;">
                <h3>موجودہ بقایا رقم: <span id="balance-page-total" style="color: #0284c7;">Rs. 0</span></h3>
            </div>
        </div>

        <!-- میرا ترقیاتی منصوبہ کا صفحہ -->
        <div id="tab-plan" class="section-card">
            <button class="btn-back" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            <h2 class="section-title">🚀 میرا ترقیاتی منصوبہ</h2>
            <p style="margin-bottom: 15px;">اپنے مالی اور ذاتی ترقیاتی اہداف یہاں درج کریں:</p>
            <div class="input-group">
                <textarea id="dev-plan-text" rows="6" placeholder="اپنے اہداف اور منصوبے یہاں لکھیں..." style="resize: vertical;"></textarea>
                <button class="btn-add" onclick="saveDevPlan()">منصوبہ محفوظ کریں</button>
            </div>
            <p id="plan-status" style="color: var(--success-color); font-weight: bold; margin-top: 10px;"></p>
        </div>

        <!-- About Us کا صفحہ -->
        <div id="tab-about" class="section-card">
            <button class="btn-back" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            <h2 class="section-title">ℹ️ ہمارے بارے میں (About Us)</h2>
            <p style="line-height: 1.8; color: #475569; font-size: 1.05rem;">
                "میرا ماہانہ حساب" ایک جدید اور آسان ڈیجیٹل مالیاتی پلیٹ فارم ہے جو آپ کو اپنے روزمرہ کے اخراجات، آمدن، اور بچت کا حساب کتاب رکھنے میں مدد فراہم کرتا ہے۔ اس کا مقصد آپ کی مالی زندگی کو منظم بنانا اور آپ کے ترقیاتی اہداف کو آسان بنانا ہے۔
            </p>
        </div>

        <!-- Privacy Policy کا صفحہ -->
        <div id="tab-privacy" class="section-card">
            <button class="btn-back" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            <h2 class="section-title">🔒 پرائیویسی پالیسی (Privacy Policy)</h2>
            <p style="line-height: 1.8; color: #475569; font-size: 1.05rem;">
                آپ کی رازداری ہمارے لیے انتہائی اہم ہے۔ اس ایپلیکیشن میں درج کیا جانے والا تمام مالیاتی ڈیٹا اور معلومات آپ کے اپنے براؤزر کے لوکل سٹوریج (Local Storage) میں محفوظ ہوتی ہیں، اور کسی بھی بیرونی سرور پر منتقل نہیں کی جاتیں۔
            </p>
        </div>

        <!-- Contact Us کا صفحہ -->
        <div id="tab-contact" class="section-card">
            <button class="btn-back" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            <h2 class="section-title">📞 رابطہ کریں (Contact Us)</h2>
            <p style="margin-bottom: 15px; color: #475569;">اگر آپ کو کوئی سوال پوچھنا ہو یا کوئی تجاویز دینی ہوں تو آپ ہم سے رابطہ کر سکتے ہیں:</p>
            <div style="background: #f8fafc; padding: 20px; border-radius: 8px; border: 1px solid #e2e8f0; font-size: 1.05rem;">
                <p><strong>ای میل:</strong> support@merasahisab.com</p>
                <p style="margin-top: 10px;"><strong>فون:</strong> +92 300 0000000</p>
            </div>
        </div>

    </div>

    <!-- 6. بڑا فوٹر -->
    <footer>
        <div class="footer-links">
            <button class="footer-btn" onclick="switchSection('tab-about')">About Us</button>
            <button class="footer-btn" onclick="switchSection('tab-privacy')">Privacy</button>
            <button class="footer-btn" onclick="switchSection('tab-contact')">Contact Us</button>
        </div>
        <p style="font-size: 0.95rem; color: #94a3b8; margin-top: 15px;">&copy; 2026 میرا ماہانہ حساب - جملہ حقوق محفوظ ہیں۔</p>
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

            document.getElementById('desc').value = '';
            document.getElementById('amount').value = '';
            
            saveData();
            
            document.getElementById('form-status').innerText = 'اندراج کامیابی سے شامل کر لیا گیا ہے!';
            setTimeout(() => {
                document.getElementById('form-status').innerText = '';
            }, 3000);
        }

        function updateDashboard() {
            const netBalance = totalIncome - totalExpense;
            
            document.getElementById('total-income').innerText = `Rs. ${totalIncome}`;
            document.getElementById('total-expense').innerText = `Rs. ${totalExpense}`;
            document.getElementById('net-balance').innerText = `Rs. ${netBalance}`;

            document.getElementById('income-page-total').innerText = `Rs. ${totalIncome}`;
            document.getElementById('expense-page-total').innerText = `Rs. ${totalExpense}`;
            document.getElementById('balance-page-total').innerText = `Rs. ${netBalance}`;
        }

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
            }
        }
    </script>
</body>
</html>
