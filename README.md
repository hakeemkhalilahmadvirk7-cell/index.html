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
            display: none;
        }

        .section-card.active-section {
            display: block;
        }

        .section-title {
            border-bottom: 3px solid var(--primary-color);
            padding-bottom: 12px;
            margin-bottom: 20px;
            color: var(--primary-color);
            font-size: 2rem;
            font-weight: bold;
        }

        /* 2. واپس جائیں والا بٹن بائیں طرف */
        .top-navigation {
            display: flex;
            justify-content: flex-end; /* RTL میں flex-end کا مطلب بائیں جانب ہوتا ہے */
            margin-bottom: 15px;
        }

        .btn-back {
            background: #64748b;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: bold;
            transition: 0.3s;
        }

        .btn-back:hover {
            background: #475569;
        }

        /* چاروں بٹن ایک ہی لائن میں */
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
            padding: 14px 8px;
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
            font-size: 1.05rem;
            font-weight: 600;
            margin-bottom: 4px;
        }

        .stat-box-btn p {
            font-size: 1.15rem;
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
            gap: 15px;
            margin-bottom: 20px;
            max-width: 600px;
        }

        input, textarea {
            padding: 14px;
            border: 1px solid #cbd5e1;
            border-radius: 8px;
            font-size: 1.05rem;
        }

        .btn-add {
            background: var(--primary-color);
            color: white;
            border: none;
            padding: 14px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            font-size: 1.1rem;
            transition: 0.3s;
        }

        .btn-add:hover {
            background: #0f766e;
        }

        /* مالیاتی گراف ایک ہی لائن میں */
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

        /* تفصیلات کی لسٹ اسٹائلنگ */
        .details-list {
            list-style: none;
            margin-top: 15px;
        }

        .details-item {
            display: flex;
            justify-content: space-between;
            padding: 12px 15px;
            background: #f8fafc;
            border: 1px solid #e2e8f0;
            border-radius: 8px;
            margin-bottom: 10px;
            font-size: 1.05rem;
        }

        /* فیملی کی تصویر */
        .family-image-container {
            margin-top: 25px;
            text-align: center;
            background: #f8fafc;
            padding: 15px;
            border-radius: 12px;
            border: 1px solid #e2e8f0;
        }

        .family-image-container img {
            max-width: 100%;
            height: auto;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }

        /* 4, 6. جدید بڑا فوٹر جو چوڑا ہے اور اوپر نیچے سے سائز کم ہے */
        footer {
            margin-top: 35px;
            background: #0f172a;
            color: white;
            padding: 20px 30px; /* اوپر نیچے سے کم سائز */
            border-radius: 12px;
            width: 100%; /* لمبائی میں بڑا */
            box-shadow: 0 -4px 15px rgba(0,0,0,0.1);
        }

        .footer-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 25px;
            align-items: center;
        }

        @media (max-width: 768px) {
            .footer-grid {
                grid-template-columns: 1fr;
            }
        }

        .footer-sec {
            background: #1e293b;
            padding: 15px;
            border-radius: 10px;
            border: 1px solid #334155;
        }

        .footer-sec h4 {
            color: #38bdf8;
            margin-bottom: 10px;
            font-size: 1.1rem;
        }

        .yt-search-box {
            display: flex;
            gap: 8px;
        }

        .yt-search-box input {
            flex: 1;
            padding: 8px 12px;
            border-radius: 6px;
            border: none;
        }

        .yt-search-box button {
            background: #ef4444;
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
        }

        .yt-links-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
        }

        .yt-link-btn {
            background: #ef4444;
            color: white;
            text-decoration: none;
            padding: 6px 12px;
            border-radius: 6px;
            font-size: 0.85rem;
            font-weight: bold;
            transition: 0.3s;
        }

        .yt-link-btn:hover {
            background: #dc2626;
        }

        .footer-bottom-links {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 15px;
            border-top: 1px solid #334155;
            padding-top: 12px;
        }

        .footer-btn {
            background: transparent;
            border: 1px solid #475569;
            color: white;
            padding: 6px 16px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 0.9rem;
        }

        .footer-btn:hover {
            background: var(--primary-color);
        }
    </style>
</head>
<body onload="initApp()">

    <div class="portal-container">

        <!-- 5. ڈیش بورڈ (اوپر والا ہیڈر ختم کر دیا گیا ہے) -->
        <div id="tab-hisab" class="section-card active-section">
            <h2 class="section-title">📊 مرکزی مالیاتی ڈیش بورڈ (میرا ماہانہ حساب)</h2>

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

            <!-- تینوں مالیاتی گراف ایک ہی لائن میں -->
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

            <!-- مالیاتی گراف کے نیچے تصویر -->
            <div class="family-image-container">
                <img src="watermarked_img_1323761495528244343.jpg" alt="">
            </div>
        </div>

        <!-- 1, 2. کل آمدن اور نیا اندراج کا پیج (بڑا ہیڈر اور بائیں طرف بٹن) -->
        <div id="tab-income-page" class="section-card">
            <!-- 2. بائیں طرف واپس جانے کا بٹن -->
            <div class="top-navigation">
                <button class="btn-back" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            </div>

            <!-- 1. بڑا ہیڈر -->
            <div style="background: var(--primary-color); color: white; padding: 35px 20px; border-radius: 12px; text-align: center; margin-bottom: 25px; box-shadow: 0 4px 10px rgba(0,0,0,0.1);">
                <h1 style="font-size: 2.8rem; font-weight: bold;">💵 کل آمدن اور نیا اندراج</h1>
            </div>

            <div style="background: #f0fdf4; border: 1px solid #bbf7d0; padding: 18px; border-radius: 8px; margin-bottom: 25px;">
                <h3 style="font-size: 1.4rem;">موجودہ کل آمدن: <span id="income-page-total" style="color: var(--success-color);">Rs. 0</span></h3>
            </div>

            <h2 style="margin-bottom: 15px; color: var(--primary-color);">📝 نیا اندراج کریں:</h2>
            <div class="input-group">
                <input type="text" id="desc" placeholder="تفصیل (مثلاً: تنخواہ، بزنس منافع، تحفہ)">
                <input type="number" id="amount" placeholder="رقم (روپے)">
                <!-- 3. شامل کریں پر نیا پیج کھلے گا -->
                <button class="btn-add" onclick="addIncomeTransaction()">شامل کریں</button>
            </div>
        </div>

        <!-- 3. نیا پیج: آمدن کی تمام تفصیلات -->
        <div id="tab-income-details" class="section-card">
            <div class="top-navigation">
                <button class="btn-back" onclick="switchSection('tab-income-page')">← واپس اندراج فارم پر جائیں</button>
            </div>
            <h2 class="section-title">📋 آمدن کی تمام تفصیلات</h2>
            <p style="font-size: 1.1rem; margin-bottom: 15px; color: #475569;">آپ کی درج کردہ تمام آمدنیوں کی لسٹ درج ذیل ہے:</p>
            
            <ul id="income-details-list" class="details-list">
                <!-- جاوا سکرپٹ کے ذریعے آمدن کی تمام لسٹ آئے گی -->
            </ul>
        </div>

        <!-- کل خرچ کا پیج -->
        <div id="tab-expense-page" class="section-card">
            <div class="top-navigation">
                <button class="btn-back" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            </div>
            <h2 class="section-title">💸 کل خرچ کی تفصیل</h2>
            <div style="background: #fff1f2; border: 1px solid #fecdd3; padding: 20px; border-radius: 8px;">
                <h3>موجودہ کل خرچہ: <span id="expense-page-total" style="color: var(--danger-color);">Rs. 0</span></h3>
            </div>
        </div>

        <!-- بقایا رقم کا پیج -->
        <div id="tab-balance-page" class="section-card">
            <div class="top-navigation">
                <button class="btn-back" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            </div>
            <h2 class="section-title">🏦 بقایا رقم (بچت)</h2>
            <div style="background: #f0f9ff; border: 1px solid #bae6fd; padding: 20px; border-radius: 8px;">
                <h3>موجودہ بقایا رقم: <span id="balance-page-total" style="color: #0284c7;">Rs. 0</span></h3>
            </div>
        </div>

        <!-- میرا ترقیاتی منصوبہ کا صفحہ -->
        <div id="tab-plan" class="section-card">
            <div class="top-navigation">
                <button class="btn-back" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            </div>
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
            <div class="top-navigation">
                <button class="btn-back" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            </div>
            <h2 class="section-title">ℹ️ ہمارے بارے میں (About Us)</h2>
            <p style="line-height: 1.8; color: #475569; font-size: 1.05rem;">
                "میرا ماہانہ حساب" ایک جدید اور آسان ڈیجیٹل مالیاتی پلیٹ فارم ہے جو آپ کو اپنے روزمرہ کے اخراجات، آمدن، اور بچت کا حساب کتاب رکھنے میں مدد فراہم کرتا ہے۔
            </p>
        </div>

        <!-- Privacy Policy کا صفحہ -->
        <div id="tab-privacy" class="section-card">
            <div class="top-navigation">
                <button class="btn-back" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            </div>
            <h2 class="section-title">🔒 پرائیویسی پالیسی (Privacy Policy)</h2>
            <p style="line-height: 1.8; color: #475569; font-size: 1.05rem;">
                آپ کی رازداری ہمارے لیے انتہائی اہم ہے۔ اس ایپلیکیشن میں درج کیا جانے والا تمام ڈیٹا آپ کے اپنے براؤزر کے لوکل سٹوریج میں محفوظ ہوتا ہے۔
            </p>
        </div>

        <!-- Contact Us کا صفحہ -->
        <div id="tab-contact" class="section-card">
            <div class="top-navigation">
                <button class="btn-back" onclick="switchSection('tab-hisab')">← واپس ڈیش بورڈ پر جائیں</button>
            </div>
            <h2 class="section-title">📞 رابطہ کریں (Contact Us)</h2>
            <div style="background: #f8fafc; padding: 20px; border-radius: 8px; border: 1px solid #e2e8f0; font-size: 1.05rem;">
                <p><strong>ای میل:</strong> support@merasahisab.com</p>
                <p style="margin-top: 10px;"><strong>فون:</strong> +92 300 0000000</p>
            </div>
        </div>

    </div>

    <!-- 4, 6. بڑا چوڑا فوٹر جس کے دو حصے ہیں -->
    <footer>
        <div class="footer-grid">
            <!-- حصہ ۱: یوٹیوب سرچ بار -->
            <div class="footer-sec">
                <h4>🔍 آمدنی بڑھانے کے لیے یوٹیوب سرچ کریں:</h4>
                <div class="yt-search-box">
                    <input type="text" id="ytQuery" placeholder="مثلاً: آمدنی بڑھانے کے طریقے...">
                    <button onclick="searchYouTube()">سرچ کریں</button>
                </div>
            </div>

            <!-- حصہ ۲: آمدنی بڑھانے والے چینلز و لنکس -->
            <div class="footer-sec">
                <h4>🎥 آمدنی بڑھانے والی ویڈیوز اور چینلز:</h4>
                <div class="yt-links-grid">
                    <a class="yt-link-btn" href="https://www.youtube.com/results?search_query=how+to+increase+income+in+urdu" target="_blank">▶ آمدنی بڑھانے کے طریقے</a>
                    <a class="yt-link-btn" href="https://www.youtube.com/results?search_query=online+earning+in+pakistan" target="_blank">▶ آن لائن کمائی</a>
                    <a class="yt-link-btn" href="https://www.youtube.com/results?search_query=business+ideas+in+urdu" target="_blank">▶ بزنس آئیڈیاز</a>
                </div>
            </div>
        </div>

        <!-- فوٹر کے نچلے لنکس -->
        <div class="footer-bottom-links">
            <button class="footer-btn" onclick="switchSection('tab-about')">About Us</button>
            <button class="footer-btn" onclick="switchSection('tab-privacy')">Privacy</button>
            <button class="footer-btn" onclick="switchSection('tab-contact')">Contact Us</button>
        </div>

        <!-- فوٹر میں تصویر -->
        <div style="max-width: 400px; margin: 12px auto 5px auto;">
            <img src="watermarked_img_1323761495528244343.jpg" alt="" style="width: 100%; border-radius: 8px; border: 2px solid #475569;">
        </div>

        <p style="font-size: 0.85rem; color: #94a3b8; text-align: center; margin-top: 8px;">&copy; 2026 میرا ماہانہ حساب - جملہ حقوق محفوظ ہیں۔</p>
    </footer>

    <script>
        let totalIncome = 0;
        let totalExpense = 0;
        let incomeList = [];
        
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

        // 3. آمدن کا اندراج اور لسٹ کا نیا پیج کھولنا
        function addIncomeTransaction() {
            const desc = document.getElementById('desc').value;
            const amount = parseFloat(document.getElementById('amount').value);

            if (!desc || isNaN(amount) || amount <= 0) {
                alert('براہ کرم صحیح تفصیل اور رقم درج کریں!');
                return;
            }

            totalIncome += amount;
            incomeList.unshift({ desc, amount, date: new Date().toLocaleDateString('ur-PK') });

            updateDashboard();
            updateCharts();
            renderIncomeDetails();

            document.getElementById('desc').value = '';
            document.getElementById('amount').value = '';
            
            saveData();

            // نیا پیج کھولیں
            switchSection('tab-income-details');
        }

        function renderIncomeDetails() {
            const listElement = document.getElementById('income-details-list');
            listElement.innerHTML = '';

            if (incomeList.length === 0) {
                listElement.innerHTML = '<li class="details-item">ابھی تک کوئی آمدن درج نہیں کی گئی۔</li>';
                return;
            }

            incomeList.forEach(item => {
                const li = document.createElement('li');
                li.className = 'details-item';
                li.innerHTML = `
                    <span><strong>${item.desc}</strong></span>
                    <span style="color: var(--success-color); font-weight: bold;">+ Rs. ${item.amount}</span>
                `;
                listElement.appendChild(li);
            });
        }

        // 6. یوٹیوب سرچ فنکشن
        function searchYouTube() {
            const query = document.getElementById('ytQuery').value;
            if(query.trim() !== "") {
                const url = `https://www.youtube.com/results?search_query=${encodeURIComponent(query)}`;
                window.open(url, '_blank');
            } else {
                alert("براہ کرم سرچ کرنے کے لیے کچھ لکھیں!");
            }
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
            const data = { totalIncome, totalExpense, incomeList, devPlan };
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
                incomeList = data.incomeList || [];
                if (data.devPlan) {
                    document.getElementById('dev-plan-text').value = data.devPlan;
                }
                renderIncomeDetails();
            }
        }
    </script>
</body>
</html>
