<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Portal 9D - Secure System</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --primary: #0077b6;
            --secondary: #00b4d8;
            --white: #ffffff;
            --bg: #caf0f8;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Poppins', sans-serif; }
        body { background: var(--bg); display: flex; min-height: 100vh; transition: 0.3s; }

        /* Login Overlay */
        #login-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%; 
            background: rgba(0,0,0,0.8); display: flex; justify-content: center; 
            align-items: center; z-index: 1000;
        }
        .login-card { background: white; padding: 30px; border-radius: 15px; text-align: center; width: 350px; }
        .google-btn { 
            background: #db4437; color: white; border: none; padding: 10px 20px; 
            border-radius: 5px; cursor: pointer; font-weight: bold; margin-top: 20px; width: 100%;
        }

        /* Sidebar */
        .sidebar { width: 260px; background: var(--primary); color: white; padding: 20px; display: flex; flex-direction: column; gap: 10px; }
        .nav-btn { 
            padding: 12px; background: rgba(255,255,255,0.1); border: none; color: white; 
            text-align: left; border-radius: 8px; cursor: pointer; 
        }
        .nav-btn:hover, .nav-btn.active { background: white; color: var(--primary); }
        .role-badge { font-size: 10px; background: gold; color: black; padding: 2px 8px; border-radius: 10px; font-weight: bold; }

        /* Content */
        .main { flex: 1; padding: 30px; overflow-y: auto; }
        .section { display: none; }
        .section.active { display: block; }
        .card { background: white; padding: 20px; border-radius: 12px; margin-bottom: 20px; box-shadow: 0 4px 6px rgba(0,0,0,0.05); }

        /* Restriction Info */
        .locked { opacity: 0.5; pointer-events: none; position: relative; }
        .locked::after { content: "🚫 Akses Terkunci"; position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); color: red; font-weight: bold; }

        /* Log Style */
        .log-box { background: #1e1e1e; color: #00ff00; padding: 15px; border-radius: 8px; font-family: monospace; height: 200px; overflow-y: auto; }
    </style>
</head>
<body>

    <div id="login-overlay">
        <div class="login-card">
            <h2>Portal Kelas 9D</h2>
            <p>Silakan pilih akun simulator</p>
            <select id="role-select" style="width: 100%; padding: 10px; margin-top: 10px;">
                <option value="Siswa">Masuk sebagai Siswa</option>
                <option value="Guru">Masuk sebagai Guru</option>
                <option value="Bendahara">Masuk sebagai Bendahara</option>
                <option value="Ketua Kelas">Masuk sebagai Ketua Kelas</option>
                <option value="Editor">Masuk sebagai Editor</option>
            </select>
            <button class="google-btn" onclick="loginSimulasi()">G Login with Google</button>
        </div>
    </div>

    <div class="sidebar">
        <h3>🌊 Dashboard 9D</h3>
        <p id="user-info">User: -</p>
        <span id="role-display" class="role-badge">Role</span>
        <hr>
        <button class="nav-btn active" onclick="showSec('home', this)">🏠 Home</button>
        <button class="nav-btn" onclick="showSec('absen', this)">📝 Absensi</button>
        <button class="nav-btn" onclick="showSec('kas', this)">💰 Kas Kelas</button>
        <button class="nav-btn" onclick="showSec('history', this)">📜 Log Aktivitas</button>
        <button class="nav-btn" onclick="showSec('settings', this)">⚙️ Editor Tampilan</button>
        <button onclick="location.reload()" style="margin-top:auto; padding: 10px; background: #e63946; color: white; border: none; border-radius: 5px;">Logout</button>
    </div>

    <div class="main">
        <div id="home" class="section active">
            <h1>Halo, <span id="welcome-name">User</span>!</h1>
            <div class="card">
                <h3>Informasi Hak Akses:</h3>
                <ul id="permission-list"></ul>
            </div>
        </div>

        <div id="absen" class="section">
            <h1>Data Absensi</h1>
            <div class="card" id="absen-container">
                <p>Klik tombol untuk ubah status (H/S/A)</p>
                <table border="1" width="100%" style="border-collapse: collapse;">
                    <tr><th>Nama</th><th>Aksi</th></tr>
                    <tr><td>Siswa 1</td><td><button onclick="logAction('Ubah Absen Siswa 1')">Edit</button></td></tr>
                </table>
            </div>
        </div>

        <div id="kas" class="section">
            <h1>Kas Kelas</h1>
            <div class="card" id="kas-container">
                <p>Total Kas: Rp 1.500.000</p>
                <button onclick="logAction('Menambah Kas Rp 50.000')">Input Kas</button>
            </div>
        </div>

        <div id="history" class="section">
            <h1>History Login & Edit</h1>
            <div class="card">
                <div class="log-box" id="log-display"></div>
            </div>
        </div>

        <div id="settings" class="section">
            <h1>Editor Tampilan</h1>
            <div class="card" id="editor-container">
                <label>Warna Tema:</label>
                <input type="color" onchange="updateTheme(this.value)">
                <p style="margin-top: 10px;">Hanya bisa diakses oleh <b>Editor, Guru, Ketua Kelas</b>.</p>
            </div>
        </div>
    </div>

    <script>
        let currentRole = "";
        let userName = "";

        // DATA ROLE (RENG)
        const rolePermissions = {
            "Guru": { canEditAbsen: true, canEditKas: true, canEditStyle: true },
            "Ketua Kelas": { canEditAbsen: true, canEditKas: true, canEditStyle: true },
            "Bendahara": { canEditAbsen: false, canEditKas: true, canEditStyle: false },
            "Editor": { canEditAbsen: true, canEditKas: true, canEditStyle: true },
            "Siswa": { canEditAbsen: false, canEditKas: false, canEditStyle: false }
        };

        function loginSimulasi() {
            currentRole = document.getElementById('role-select').value;
            userName = "User_" + Math.floor(Math.random() * 1000);
            
            document.getElementById('login-overlay').style.display = 'none';
            document.getElementById('user-info').innerText = "User: " + userName;
            document.getElementById('welcome-name').innerText = userName;
            document.getElementById('role-display').innerText = currentRole;
            
            applyPermissions();
            logAction("Login ke sistem");
        }

        function applyPermissions() {
            const p = rolePermissions[currentRole];
            
            // Atur Absen
            if (!p.canEditAbsen) document.getElementById('absen-container').classList.add('locked');
            else document.getElementById('absen-container').classList.remove('locked');

            // Atur Kas
            if (!p.canEditKas) document.getElementById('kas-container').classList.add('locked');
            else document.getElementById('kas-container').classList.remove('locked');

            // Atur Settings
            if (!p.canEditStyle) document.getElementById('editor-container').classList.add('locked');
            else document.getElementById('editor-container').classList.remove('locked');
            
            // Isi List Izin
            const list = document.getElementById('permission-list');
            list.innerHTML = `
                <li>Edit Absen: ${p.canEditAbsen ? '✅' : '❌'}</li>
                <li>Edit Kas: ${p.canEditKas ? '✅' : '❌'}</li>
                <li>Edit Tampilan: ${p.canEditStyle ? '✅' : '❌'}</li>
            `;
        }

        function showSec(id, btn) {
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            btn.classList.add('active');
        }

        function logAction(action) {
            const logBox = document.getElementById('log-display');
            const now = new Date().toLocaleTimeString();
            const logEntry = `<div>[${now}] <b>${userName} (${currentRole})</b>: ${action}</div>`;
            logBox.innerHTML = logEntry + logBox.innerHTML;
            
            // Simpan ke local storage agar tidak hilang saat refresh
            let logs = JSON.parse(localStorage.getItem('app_logs') || "[]");
            logs.unshift(`[${now}] ${userName} (${currentRole}): ${action}`);
            localStorage.setItem('app_logs', JSON.stringify(logs.slice(0, 50)));
        }

        function updateTheme(color) {
            document.documentElement.style.setProperty('--primary', color);
            logAction("Mengubah warna tema ke " + color);
        }

        // Load logs on start
        window.onload = () => {
            const savedLogs = JSON.parse(localStorage.getItem('app_logs') || "[]");
            const logBox = document.getElementById('log-display');
            savedLogs.forEach(l => logBox.innerHTML += `<div>${l}</div>`);
        };
    </script>
</body>
</html>
