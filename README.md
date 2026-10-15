<!DOCTYPE html>
<html lang="ur" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>میرا ماہانہ حساب</title>
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

        /* Nav Tabs for Mobile & Desktop */
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
            padding: 12px 20px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            font-size: 1rem;
            transition: 0.3s;
        }

        .tab-btn.active {
            background: var(--primary-color);
            color: white;
        }

        /* Layout Grid */
        .portal-container {
            display: grid;
            grid-template-columns: 1fr;
            gap: 20px;
        }

        /* Show grid layout on big screens */
        @media (min-width: 1024px) {
            .portal-container {
                grid-template-columns: 2fr 1fr;
            }
            .tab-btn {
                display: none; /* Hide tab buttons on large desktop screens */
            }
            .section-card {
                display: block !important;
            }
        }

        .section-card {
            background: var(--card-bg);
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
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

        /* Folders / Categories Section */
        .folder-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
            gap: 10px;
            margin-top: 15px;
        }

        .folder-box {
            border: 1px dashed var(--primary-color);
            padding: 12px;
            border-radius: 8px;
            text-align: center;
            background: #f8fafc;
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
<body onload="loadData()">

    <header>
        <h1>میرا ماہانہ حساب</h1>
    </header>

    <!-- Mobile Navigation Tabs -->
    <div class="nav-tabs">
        <button class="tab-btn active" onclick="switchTab('tab-hisab')">۱. میرا حساب</button>
        <button class="tab-btn" onclick="switchTab('tab-plan')">۲. میرا ترقیاتی منصوبہ</button>
    </div>

    <div class="portal-container">

        <!-- SECTION 1: میرا حساب (Calculator & Folders) -->
        <div id="tab-hisab" class="section-card">
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

            <!-- Transaction Form -->
            <h3>نیا اندراج کریں:</h3>
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

            <!-- Custom Folders Section -->
            <h3>📁 حساب کے فولڈرز (کاؤنٹرز):</h3>
            <div class="folder-grid">
                <div class="folder-box">
                    <strong>گروسری</strong>
                    <p id="f-grocery">Rs. 0</p>
                </div>
                <div class="folder-box">
                    <strong>یوٹیلٹی بلز</strong>
                    <p id="f-bills">Rs. 0</p>
                </div>
                <div class="folder-box">
                    <strong>صحت و ادویات</strong>
                    <p id="f-health">Rs. 0</p>
                </div>
            </div>

            <h3 style="margin-top: 20px;">حالیہ تاریخچہ (History):</h3>
            <ul id="history" class="history-list"></ul>
        </div>

        <!-- SECTION 2: میرا ترقیاتی منصوبہ -->
        <div id="tab-plan" class="section-card" style="display: none;">
            <h2 class="section-title">🚀 میرا ترقیاتی منصوبہ</h2>
            <p style="margin-bottom: 15px;">اپنے مالی اور ذاتی ترقیاتی اہداف یہاں درج کریں:</p>
            <div class="input-group">
                <textarea id="dev-plan-text" rows="6" placeholder="اپنے اہداف اور منصوبے یہاں لکھیں..." style="resize: vertical;"></textarea>
                <button class="btn-add" onclick="saveDevPlan()">منصوبہ محفوظ کریں</button>
            </div>
            <p id="plan-status" style="color: var(--success-color); font-weight: bold; margin-top: 10px;"></p>
        </div>

    </div>

    <script>
        let totalIncome = 0;
        let totalExpense = 0;
        let folderTotals = {
            'گروسری': 0,
            'یوٹیلٹی بلز': 0,
            'صحت و ادویات': 0
        };

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
                if (folderTotals[folder] !== undefined) {
                    folderTotals[folder] += amount;
                }
            }

            updateDashboard();

            // Add to history
            const historyList = document.getElementById('history');
            const li = document.createElement('li');
            li.className = 'history-item';
            li.innerHTML = `<span>${desc} (${folder})</span> <strong style="color: ${type === 'income' ? 'green' : 'red'};">${type === 'income' ? '+' : '-'} Rs. ${amount}</strong>`;
            historyList.prepend(li);

            // Clear inputs
            document.getElementById('desc').value = '';
            document.getElementById('amount').value = '';
            
            // Save to memory
            saveData();
        }

        function updateDashboard() {
            document.getElementById('total-income').innerText = `Rs. ${totalIncome}`;
            document.getElementById('total-expense').innerText = `Rs. ${totalExpense}`;
            document.getElementById('net-balance').innerText = `Rs. ${totalIncome - totalExpense}`;

            if(document.getElementById('f-grocery')) document.getElementById('f-grocery').innerText = `Rs. ${folderTotals['گروسری']}`;
            if(document.getElementById('f-bills')) document.getElementById('f-bills').innerText = `Rs. ${folderTotals['یوٹیلٹی بلز']}`;
            if(document.getElementById('f-health')) document.getElementById('f-health').innerText = `Rs. ${folderTotals['صحت و ادویات']}`;
        }

        function switchTab(tabId) {
            document.getElementById('tab-hisab').style.display = 'none';
            document.getElementById('tab-plan').style.display = 'none';

            document.getElementById(tabId).style.display = 'block';

            const buttons = document.querySelectorAll('.tab-btn');
            buttons.forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
        }

        function saveData() {
            const devPlan = document.getElementById('dev-plan-text').value;
            const data = { totalIncome, totalExpense, folderTotals, devPlan };
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
                folderTotals = data.folderTotals || folderTotals;
                if (data.devPlan) {
                    document.getElementById('dev-plan-text').value = data.devPlan;
                }
                updateDashboard();
            }
        }
    </script>
</body>
</html>
