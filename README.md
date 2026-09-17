<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>منصة تشكيل</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); min-height: 100vh; display: flex; align-items: center; justify-content: center; }
        .container { width: 100%; max-width: 1200px; padding: 20px; }
        .login-container { background: white; border-radius: 15px; box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2); padding: 40px; max-width: 500px; margin: 0 auto; }
        .login-header { text-align: center; margin-bottom: 40px; }
        .login-header h1 { color: #333; font-size: 28px; margin-bottom: 10px; }
        .login-header p { color: #666; font-size: 14px; }
        .form-group { margin-bottom: 20px; }
        label { display: block; color: #333; font-weight: 600; margin-bottom: 8px; font-size: 14px; }
        input { width: 100%; padding: 12px; border: 2px solid #e0e0e0; border-radius: 8px; font-size: 14px; }
        input:focus { outline: none; border-color: #667eea; }
        .btn-login { width: 100%; padding: 12px; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; border: none; border-radius: 8px; font-size: 16px; font-weight: 600; cursor: pointer; }
        .btn-login:hover { transform: translateY(-2px); }
        .hidden { display: none; }
        .dashboard { background: white; border-radius: 15px; box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2); padding: 30px; }
        .dashboard-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 30px; border-bottom: 2px solid #f0f0f0; padding-bottom: 20px; }
        .dashboard-header h2 { color: #333; font-size: 24px; }
        .btn-logout { padding: 8px 20px; background: #f44336; color: white; border: none; border-radius: 6px; cursor: pointer; }
        .tabs { display: flex; gap: 10px; margin-bottom: 20px; border-bottom: 2px solid #f0f0f0; }
        .tab-btn { padding: 10px 20px; background: none; border: none; cursor: pointer; color: #666; border-bottom: 3px solid transparent; }
        .tab-btn.active { color: #667eea; border-bottom-color: #667eea; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        th, td { padding: 12px; text-align: right; border-bottom: 1px solid #f0f0f0; font-size: 14px; }
        th { background: #f5f5f5; font-weight: 600; }
        .status-badge { display: inline-block; padding: 4px 12px; border-radius: 20px; font-size: 12px; font-weight: 600; }
        .status-new { background: #e3f2fd; color: #1976d2; }
        .status-complete { background: #e8f5e9; color: #388e3c; }
    </style>
</head>
<body>
    <div class="container">
        <div class="login-container" id="loginSection">
            <div class="login-header">
                <h1>🎨 منصة تشكيل</h1>
                <p>لوحة التحكم الداخلية</p>
            </div>
            <form id="loginForm">
                <div class="form-group">
                    <label>اسم المستخدم</label>
                    <input type="text" id="username" required placeholder="أدخل اسم المستخدم">
                </div>
                <div class="form-group">
                    <label>كلمة المرور</label>
                    <input type="password" id="password" required placeholder="أدخل كلمة المرور">
                </div>
                <button type="submit" class="btn-login">دخول</button>
            </form>
        </div>

        <div id="dashboardSection" class="hidden">
            <div class="dashboard">
                <div class="dashboard-header">
                    <h2>مرحباً، <span id="usernameDashboard">المستخدم</span></h2>
                    <button class="btn-logout" onclick="logout()">تسجيل خروج</button>
                </div>

                <div class="tabs">
                    <button class="tab-btn active" onclick="switchTab('customers')">العملاء</button>
                    <button class="tab-btn" onclick="switchTab('requests')">الطلبات</button>
                    <button class="tab-btn" onclick="switchTab('purchases')">المشتريات</button>
                </div>

                <div id="customers" class="tab-content active">
                    <table>
                        <thead><tr><th>رقم التواصل</th><th>اسم العميل</th><th>رقم العميل</th></tr></thead>
                        <tbody id="customersBody"></tbody>
                    </table>
                </div>

                <div id="requests" class="tab-content">
                    <table>
                        <thead><tr><th>الحالة</th><th>التاريخ</th><th>الطلب</th><th>رقم العميل</th><th>رقم الطلب</th></tr></thead>
                        <tbody id="requestsBody"></tbody>
                    </table>
                </div>

                <div id="purchases" class="tab-content">
                    <table>
                        <thead><tr><th>الحالة</th><th>التاريخ</th><th>المادة</th><th>المورد</th><th>رقم المشتريات</th></tr></thead>
                        <tbody id="purchasesBody"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <script>
        const data = {
            customers: [
                { id: '1', name: 'معاذ', phone: '593078804' }
            ],
            requests: [
                { id: '1', customerId: '1', desc: 'ديكور', date: '17-09-2026', status: 'جديد' }
            ],
            purchases: [
                { id: '1', supplier: 'ديكورات صح', material: 'بطل شوود', date: '17-09-2026', status: 'تم التوصيل' }
            ]
        };

        document.getElementById('loginForm').addEventListener('submit', function(e) {
            e.preventDefault();
            if (document.getElementById('username').value === 'admin' && document.getElementById('password').value === '1234') {
                login('admin');
            } else {
                alert('بيانات خاطئة');
            }
        });

        function login(username) {
            document.getElementById('loginSection').classList.add('hidden');
            document.getElementById('dashboardSection').classList.remove('hidden');
            document.getElementById('usernameDashboard').textContent = username;
            loadData();
        }

        function logout() {
            document.getElementById('loginSection').classList.remove('hidden');
            document.getElementById('dashboardSection').classList.add('hidden');
        }

        function switchTab(tab) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));
            document.getElementById(tab).classList.add('active');
            event.target.classList.add('active');
        }

        function loadData() {
            let html = '';
            data.customers.forEach(c => {
                html += `<tr><td>${c.phone}</td><td>${c.name}</td><td>${c.id}</td></tr>`;
            });
            document.getElementById('customersBody').innerHTML = html;

            html = '';
            data.requests.forEach(r => {
                html += `<tr><td><span class="status-badge status-new">${r.status}</span></td><td>${r.date}</td><td>${r.desc}</td><td>${r.customerId}</td><td>${r.id}</td></tr>`;
            });
            document.getElementById('requestsBody').innerHTML = html;

            html = '';
            data.purchases.forEach(p => {
                html += `<tr><td><span class="status-badge status-complete">${p.status}</span></td><td>${p.date}</td><td>${p.material}</td><td>${p.supplier}</td><td>${p.id}</td></tr>`;
            });
            document.getElementById('purchasesBody').innerHTML = html;
        }
    </script>
</body>
</html>
