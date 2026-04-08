<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Portal Kelas 9D - Ocean Theme</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --primary: #0077b6;
            --secondary: #00b4d8;
            --accent: #90e0ef;
            --background: #caf0f8;
            --white: rgba(255, 255, 255, 0.9);
            --text-dark: #023e8a;
            --success: #2a9d8f;
            --danger: #e63946;
            --warning: #00008b; /* Biru Gelap untuk Sakit */
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Poppins', sans-serif; }

        body {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 50%, var(--accent) 100%);
            background-attachment: fixed; color: var(--text-dark); min-height: 100vh; display: flex;
        }

        /* Sidebar Navigation */
        .sidebar {
            width: 250px; background: rgba(255, 255, 255, 0.15); backdrop-filter: blur(10px);
            border-right: 1px solid rgba(255, 255, 255, 0.3); padding: 2rem; display: flex;
            flex-direction: column; gap: 1rem;
        }

        .sidebar h2 { color: white; text-align: center; margin-bottom: 2rem; font-weight: 600; text-shadow: 0 2px 4px rgba(0,0,0,0.2); }

        .nav-btn {
            padding: 12px 20px; background: rgba(255, 255, 255, 0.8); border: none; border-radius: 10px;
            cursor: pointer; font-size: 16px; font-weight: 500; color: var(--primary);
            transition: all 0.3s ease; text-align: left;
        }

        .nav-btn:hover, .nav-btn.active {
            background: var(--primary); color: white; box-shadow: 0 4px 15px rgba(0, 119, 182, 0.3); transform: translateX(5px);
        }

        /* Main Content */
        .main-content { flex: 1; padding: 2rem; overflow-y: auto; height: 100vh; }

        .section {
            display: none; background: var(--white); backdrop-filter: blur(10px); border-radius: 20px;
            padding: 2rem; box-shadow: 0 10px 30px rgba(0,0,0,0.1); animation: fadeIn 0.4s ease-in-out;
        }
        .section.active { display: block; }

        h1 { color: var(--primary); margin-bottom: 1.5rem; border-bottom: 2px solid var(--accent); padding-bottom: 10px; }

        /* Dashboard Charts Container */
        .charts-container {
            display: flex; gap: 20px; margin-bottom: 2rem; flex-wrap: wrap;
        }
        
        .chart-box {
            flex: 1; min-width: 250px; background: white; padding: 20px; border-radius: 15px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05); border-top: 5px solid var(--secondary);
            display: flex; flex-direction: column; align-items: center; justify-content: center;
        }

        .chart-box h3 { color: var(--text-dark); margin-bottom: 10px; font-size: 16px; text-align: center;}
        .chart-box .persen-teks { font-size: 28px; font-weight: bold; color: var(--primary); margin-top: 10px; }
        
        /* Canvas untuk diagram agar tidak terlalu besar */
        .canvas-wrapper { width: 180px; height: 180px; } 

        /* Tables */
        .table-container { overflow-x: auto; }
        table { width: 100%; border-collapse: collapse; margin-top: 1rem; background: white; border-radius: 10px; overflow: hidden; font-size: 14px; }
        th, td { padding: 10px 12px; text-align: left; border-bottom: 1px solid #eee; vertical-align: middle; }
        th { background-color: var(--primary); color: white; font-weight: 500; }
        tr:hover { background-color: #f8f9fa; }

        /* Forms & Inputs */
        input[type="text"], input[type="number"], input[type="date"] {
            width: 100%; padding: 10px; border: 1px solid #ccc; border-radius: 8px; font-family: inherit;
        }
        .input-small { width: 60px !important; padding: 8px !important; text-align: center;}
        
        .radio-group { display: flex; gap: 8px; flex-wrap: wrap; }
        .radio-group label { cursor: pointer; display: flex; align-items: center; gap: 3px; font-size: 13px; }

        .settings-box {
            background: rgba(0, 180, 216, 0.1); padding: 15px; border-radius: 10px; margin-bottom: 15px;
            display: flex; align-items: center; gap: 10px; border: 1px dashed var(--primary); flex-wrap: wrap;
        }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* Mobile Responsive */
        @media (max-width: 768px) {
            body { flex-direction: column; }
            .sidebar { width: 100%; border-right: none; flex-direction: row; flex-wrap: wrap; justify-content: center; padding: 1rem; }
            .sidebar h2 { width: 100%; margin-bottom: 0.5rem; }
            .main-content { padding: 1rem; }
        }
    </style>
</head>
<body>

    <div class="sidebar">
        <h2>🌊 Kelas 9D</h2>
        <button class="nav-btn active" onclick="showSection('dashboard')">🏠 Dashboard</button>
        <button class="nav-btn" onclick="showSection('absen')">📝 Absen</button>
        <button class="nav-btn" onclick="showSection('kas')">💰 Kas</button>
        <button class="nav-btn" onclick="showSection('jurnal')">📖 Jurnal</button>
    </div>

    <div class="main-content">
        
        <div id="dashboard" class="section active">
            <h1>Dashboard Analytics</h1>
            
            <div class="charts-container">
                <div class="chart-box">
                    <h3>Kehadiran Kelas (1 Semester)</h3>
                    <div class="canvas-wrapper">
                        <canvas id="absenChart"></canvas>
                    </div>
                    <div class="persen-teks" id="teks-persen-absen">0%</div>
                </div>

                <div class="chart-box">
                    <h3>Pencapaian Kas (1 Semester)</h3>
                    <div class="canvas-wrapper">
                        <canvas id="kasChart"></canvas>
                    </div>
                    <div class="persen-teks" id="teks-persen-kas">0%</div>
                </div>
            </div>

            <div style="background: white; padding: 15px; border-radius: 10px; margin-bottom: 20px; box-shadow: 0 4px 10px rgba(0,0,0,0.05);">
                <h3 style="color: var(--primary); margin-bottom: 10px;">Rekap Absen Hari Ini:</h3>
                <div style="display: flex; gap: 20px; font-weight: bold; flex-wrap: wrap;">
                    <span style="color: var(--success);">Hadir: <span id="count-hadir">0</span></span>
                    <span style="color: #f4a261;">Sakit: <span id="count-sakit">0</span></span>
                    <span style="color: #2a9d8f;">Izin: <span id="count-izin">0</span></span>
                    <span style="color: var(--danger);">Alpa: <span id="count-alpa">0</span></span>
                </div>
            </div>
        </div>

        <div id="absen" class="section">
            <h1>Buku Absensi Semester</h1>
            
            <div class="settings-box">
                <label style="font-weight: bold;">Total Hari Efektif 1 Semester:</label>
                <input type="number" id="total-hari-semester" class="input-small" value="100" oninput="hitungSemuaSemester()">
                <span>Hari</span>
            </div>
            
            <p style="font-size: 12px; color: #666; margin-bottom: 10px;">
                *Catatan: Nama berwarna <b style="color:red;">Merah</b> = Alpa > 5. Nama berwarna <b style="color:darkblue;">Biru Gelap</b> = Sakit > 5.
            </p>

            <div class="table-container">
                <table id="tabel-absen">
                    <thead>
                        <tr>
                            <th>No</th>
                            <th>Nama Siswa</th>
                            <th>Harian</th>
                            <th>Total Hadir</th>
                            <th>Total Sakit</th>
                            <th>Total Alpa</th>
                            <th>% Smstr</th>
                        </tr>
                    </thead>
                    <tbody id="body-absen">
                        </tbody>
                </table>
            </div>
        </div>

        <div id="kas" class="section">
            <h1>Buku Kas Semester</h1>
            
            <div class="settings-box">
                <label style="font-weight: bold;">Target Kas Per Siswa (1 Smstr): Rp</label>
                <input type="number" id="target-kas-siswa" style="width: 120px; padding: 8px;" value="150000" oninput="prosesSemuaKas()">
                <div style="margin-left: auto; font-weight: bold; color: var(--primary);">
                    Terkumpul: <span id="total-kas-rp">Rp 0</span>
                </div>
            </div>

            <div class="table-container">
                <table id="tabel-kas">
                    <thead>
                        <tr>
                            <th>No</th>
                            <th>Nama Siswa</th>
                            <th>Nominal Bayar (Rp)</th>
                            <th>Kas Harian (Auto)</th>
                        </tr>
                    </thead>
                    <tbody id="body-kas">
                        </tbody>
                </table>
            </div>
        </div>

        <div id="jurnal" class="section">
            <h1>Jurnal Kelas</h1>
            <div style="margin-bottom: 15px;">
                <label>Tanggal</label>
                <input type="date" id="jurnal-tanggal">
            </div>
            <div style="margin-bottom: 15px;">
                <label>Mata Pelajaran & Guru</label>
                <input type="text" placeholder="Contoh: Matematika - Pak Budi">
            </div>
            <button style="background: var(--primary); color: white; padding: 12px 25px; border: none; border-radius: 8px; cursor: pointer; font-weight: bold;" onclick="alert('Jurnal berhasil disimpan!')">Simpan Jurnal</button>
        </div>

    </div>

    <script>
        const siswa = [
            "Agung Ramaindra Syahputra", "Ainun Tiyas martiningsih", "Alessia Alzena Riangga",
            "Almaira Faiqha", "Alya Faliha", "Amirah Julveni Maulana", "Andi Aqil",
            "Aurora Felicia Angelie", "Bilqis Melani", "Chaya muja syatira",
            "Fadhil oktavian nabil", "fahmi firjatullah", "faiz Alfarizi",
            "farhan umaydillah radjiansyah", "fathir pradipta alfarizi", "fazril nizart",
            "januari", "jihaan kaltsum khairunnisa", "juan edra abiya",
            "Khanza Rista Bhanuwati", "Kristiana Berta", "Marselino Prasetyo wijaya",
            "M. Fadil Putra pratama", "M. Kafka adri Al-Maliq 67 sigmo", "M. Luthfi faturrahman",
            "Natalis Fedora Purba", "Rafif Rakha Arkana Dih 🥀", "Rayhan Qolbu",
            "Runika Alzariva", "Suci Pratiwi", "Syafa Damia Sakhi", "Syech Faris maulana",
            "Tegar Tri Pambudi", "Tersa Usela", "Tiara Husna Humairah", "Zhafira Mayarista"
        ];
        
        const totalSiswa = siswa.length;
        let chartAbsenInst, chartKasInst;

        // Inisialisasi Chart.js
        window.onload = function() {
            // Set Tanggal Jurnal
            document.getElementById('jurnal-tanggal').valueAsDate = new Date();

            // Chart Kehadiran
            const ctxAbsen = document.getElementById('absenChart').getContext('2d');
            chartAbsenInst = new Chart(ctxAbsen, {
                type: 'doughnut',
                data: {
                    labels: ['Hadir', 'Tidak Hadir'],
                    datasets: [{
                        data: [0, 100],
                        backgroundColor: ['#2a9d8f', '#e63946'],
                        borderWidth: 0
                    }]
                },
                options: { responsive: true, maintainAspectRatio: true, plugins: { legend: { position: 'bottom' } } }
            });

            // Chart Kas
            const ctxKas = document.getElementById('kasChart').getContext('2d');
            chartKasInst = new Chart(ctxKas, {
                type: 'doughnut',
                data: {
                    labels: ['Terkumpul', 'Kekurangan'],
                    datasets: [{
                        data: [0, 100],
                        backgroundColor: ['#0077b6', '#caf0f8'],
                        borderWidth: 0
                    }]
                },
                options: { responsive: true, maintainAspectRatio: true, plugins: { legend: { position: 'bottom' } } }
            });

            RenderTabel();
        };

        function showSection(sectionId) {
            document.querySelectorAll('.section').forEach(sec => sec.classList.remove('active'));
            document.querySelectorAll('.nav-btn').forEach(btn => btn.classList.remove('active'));
            document.getElementById(sectionId).classList.add('active');
            event.currentTarget.classList.add('active');
        }

        // Render Tabel Absen & Kas
        function RenderTabel() {
            const bodyAbsen = document.getElementById('body-absen');
            const bodyKas = document.getElementById('body-kas');

            siswa.forEach((nama, index) => {
                // Tabel Absen
                let trA = document.createElement('tr');
                trA.innerHTML = `
                    <td>${index + 1}</td>
                    <td id="nama_siswa_${index}" style="font-weight: 500;">${nama}</td>
                    <td>
                        <div class="radio-group">
                            <label><input type="radio" name="absen_hari_${index}" value="H" onchange="hitungHariIni()">H</label>
                            <label><input type="radio" name="absen_hari_${index}" value="S" onchange="hitungHariIni()">S</label>
                            <label><input type="radio" name="absen_hari_${index}" value="I" onchange="hitungHariIni()">I</label>
                            <label><input type="radio" name="absen_hari_${index}" value="A" onchange="hitungHariIni()">A</label>
                        </div>
                    </td>
                    <td><input type="number" class="input-small" id="hadir_smstr_${index}" value="0" oninput="hitungSemuaSemester()"></td>
                    <td><input type="number" class="input-small" id="sakit_smstr_${index}" value="0" oninput="hitungSemuaSemester()"></td>
                    <td><input type="number" class="input-small" id="alpa_smstr_${index}" value="0" oninput="hitungSemuaSemester()"></td>
                    <td><span id="persen_smstr_${index}" style="font-weight:bold;">0%</span></td>
                `;
                bodyAbsen.appendChild(trA);

                // Tabel Kas
                let checkboxes = '';
                for(let i=1; i<=31; i++) {
                    checkboxes += `<input type="checkbox" id="kas_harian_${index}_${i}" disabled style="width: 15px; height: 15px;">`;
                }

                let trK = document.createElement('tr');
                trK.innerHTML = `
                    <td>${index + 1}</td>
                    <td>${nama}</td>
                    <td><input type="number" id="kas_bulanan_${index}" style="width: 100px; padding: 8px;" value="0" oninput="prosesSemuaKas()"></td>
                    <td><div style="display: flex; gap: 3px; flex-wrap: wrap;">${checkboxes}</div></td>
                `;
                bodyKas.appendChild(trK);
            });
        }

        // LOGIKA ABSENSI
        function hitungHariIni() {
            let h = 0, s = 0, i = 0, a = 0;
            for(let x=0; x<totalSiswa; x++) {
                let radio = document.querySelector(`input[name="absen_hari_${x}"]:checked`);
                if(radio) {
                    if(radio.value === "H") h++;
                    if(radio.value === "S") s++;
                    if(radio.value === "I") i++;
                    if(radio.value === "A") a++;
                }
            }
            document.getElementById('count-hadir').innerText = h;
            document.getElementById('count-sakit').innerText = s;
            document.getElementById('count-izin').innerText = i;
            document.getElementById('count-alpa').innerText = a;
        }

        function hitungSemuaSemester() {
            let totalHariSmstr = parseInt(document.getElementById('total-hari-semester').value) || 1;
            let totalHadirKelas = 0;

            for(let i=0; i<totalSiswa; i++) {
                let hadir = parseInt(document.getElementById(`hadir_smstr_${i}`).value) || 0;
                let sakit = parseInt(document.getElementById(`sakit_smstr_${i}`).value) || 0;
                let alpa = parseInt(document.getElementById(`alpa_smstr_${i}`).value) || 0;

                let persenIndividu = (hadir / totalHariSmstr) * 100;
                if(persenIndividu > 100) persenIndividu = 100;
                
                let namaCell = document.getElementById(`nama_siswa_${i}`);
                let persenCell = document.getElementById(`persen_smstr_${i}`);
                
                persenCell.innerText = persenIndividu.toFixed(1) + '%';
                totalHadirKelas += hadir;

                // Logika Warna Nama Siswa
                if (alpa > 5) {
                    namaCell.style.color = "var(--danger)"; // Merah
                } else if (sakit > 5) {
                    namaCell.style.color = "var(--warning)"; // Biru Gelap
                } else {
                    namaCell.style.color = "var(--text-dark)"; // Normal
                }
            }

            // Update Chart Absen di Dashboard
            let rataRataKelas = (totalHadirKelas / (totalHariSmstr * totalSiswa)) * 100;
            if(rataRataKelas > 100) rataRataKelas = 100;

            document.getElementById('teks-persen-absen').innerText = rataRataKelas.toFixed(1) + '%';
            chartAbsenInst.data.datasets[0].data = [rataRataKelas, 100 - rataRataKelas];
            chartAbsenInst.update();
        }

        // LOGIKA KAS
        function prosesSemuaKas() {
            let targetPerSiswa = parseInt(document.getElementById('target-kas-siswa').value) || 1;
            let targetTotalKelas = targetPerSiswa * totalSiswa;
            let totalTerkumpul = 0;

            for(let i=0; i<totalSiswa; i++) {<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Portal Kelas 9D - Ocean Theme</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --primary: #0077b6;
            --secondary: #00b4d8;
            --accent: #90e0ef;
            --background: #caf0f8;
            --white: rgba(255, 255, 255, 0.9);
            --text-dark: #023e8a;
            --success: #2a9d8f;
            --danger: #e63946;
            --warning: #00008b; /* Biru Gelap untuk Sakit */
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Poppins', sans-serif; }

        body {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 50%, var(--accent) 100%);
            background-attachment: fixed; color: var(--text-dark); min-height: 100vh; display: flex;
        }

        /* Sidebar Navigation */
        .sidebar {
            width: 250px; background: rgba(255, 255, 255, 0.15); backdrop-filter: blur(10px);
            border-right: 1px solid rgba(255, 255, 255, 0.3); padding: 2rem; display: flex;
            flex-direction: column; gap: 1rem;
        }

        .sidebar h2 { color: white; text-align: center; margin-bottom: 2rem; font-weight: 600; text-shadow: 0 2px 4px rgba(0,0,0,0.2); }

        .nav-btn {
            padding: 12px 20px; background: rgba(255, 255, 255, 0.8); border: none; border-radius: 10px;
            cursor: pointer; font-size: 16px; font-weight: 500; color: var(--primary);
            transition: all 0.3s ease; text-align: left;
        }

        .nav-btn:hover, .nav-btn.active {
            background: var(--primary); color: white; box-shadow: 0 4px 15px rgba(0, 119, 182, 0.3); transform: translateX(5px);
        }

        /* Main Content */
        .main-content { flex: 1; padding: 2rem; overflow-y: auto; height: 100vh; }

        .section {
            display: none; background: var(--white); backdrop-filter: blur(10px); border-radius: 20px;
            padding: 2rem; box-shadow: 0 10px 30px rgba(0,0,0,0.1); animation: fadeIn 0.4s ease-in-out;
        }
        .section.active { display: block; }

        h1 { color: var(--primary); margin-bottom: 1.5rem; border-bottom: 2px solid var(--accent); padding-bottom: 10px; }

        /* Dashboard Charts Container */
        .charts-container {
            display: flex; gap: 20px; margin-bottom: 2rem; flex-wrap: wrap;
        }
        
        .chart-box {
            flex: 1; min-width: 250px; background: white; padding: 20px; border-radius: 15px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05); border-top: 5px solid var(--secondary);
            display: flex; flex-direction: column; align-items: center; justify-content: center;
        }

        .chart-box h3 { color: var(--text-dark); margin-bottom: 10px; font-size: 16px; text-align: center;}
        .chart-box .persen-teks { font-size: 28px; font-weight: bold; color: var(--primary); margin-top: 10px; }
        
        /* Canvas untuk diagram agar tidak terlalu besar */
        .canvas-wrapper { width: 180px; height: 180px; } 

        /* Tables */
        .table-container { overflow-x: auto; }
        table { width: 100%; border-collapse: collapse; margin-top: 1rem; background: white; border-radius: 10px; overflow: hidden; font-size: 14px; }
        th, td { padding: 10px 12px; text-align: left; border-bottom: 1px solid #eee; vertical-align: middle; }
        th { background-color: var(--primary); color: white; font-weight: 500; }
        tr:hover { background-color: #f8f9fa; }

        /* Forms & Inputs */
        input[type="text"], input[type="number"], input[type="date"] {
            width: 100%; padding: 10px; border: 1px solid #ccc; border-radius: 8px; font-family: inherit;
        }
        .input-small { width: 60px !important; padding: 8px !important; text-align: center;}
        
        .radio-group { display: flex; gap: 8px; flex-wrap: wrap; }
        .radio-group label { cursor: pointer; display: flex; align-items: center; gap: 3px; font-size: 13px; }

        .settings-box {
            background: rgba(0, 180, 216, 0.1); padding: 15px; border-radius: 10px; margin-bottom: 15px;
            display: flex; align-items: center; gap: 10px; border: 1px dashed var(--primary); flex-wrap: wrap;
        }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* Mobile Responsive */
        @media (max-width: 768px) {
            body { flex-direction: column; }
            .sidebar { width: 100%; border-right: none; flex-direction: row; flex-wrap: wrap; justify-content: center; padding: 1rem; }
            .sidebar h2 { width: 100%; margin-bottom: 0.5rem; }
            .main-content { padding: 1rem; }
        }
    </style>
</head>
<body>

    <div class="sidebar">
        <h2>🌊 Kelas 9D</h2>
        <button class="nav-btn active" onclick="showSection('dashboard')">🏠 Dashboard</button>
        <button class="nav-btn" onclick="showSection('absen')">📝 Absen</button>
        <button class="nav-btn" onclick="showSection('kas')">💰 Kas</button>
        <button class="nav-btn" onclick="showSection('jurnal')">📖 Jurnal</button>
    </div>

    <div class="main-content">
        
        <div id="dashboard" class="section active">
            <h1>Dashboard Analytics</h1>
            
            <div class="charts-container">
                <div class="chart-box">
                    <h3>Kehadiran Kelas (1 Semester)</h3>
                    <div class="canvas-wrapper">
                        <canvas id="absenChart"></canvas>
                    </div>
                    <div class="persen-teks" id="teks-persen-absen">0%</div>
                </div>

                <div class="chart-box">
                    <h3>Pencapaian Kas (1 Semester)</h3>
                    <div class="canvas-wrapper">
                        <canvas id="kasChart"></canvas>
                    </div>
                    <div class="persen-teks" id="teks-persen-kas">0%</div>
                </div>
            </div>

            <div style="background: white; padding: 15px; border-radius: 10px; margin-bottom: 20px; box-shadow: 0 4px 10px rgba(0,0,0,0.05);">
                <h3 style="color: var(--primary); margin-bottom: 10px;">Rekap Absen Hari Ini:</h3>
                <div style="display: flex; gap: 20px; font-weight: bold; flex-wrap: wrap;">
                    <span style="color: var(--success);">Hadir: <span id="count-hadir">0</span></span>
                    <span style="color: #f4a261;">Sakit: <span id="count-sakit">0</span></span>
                    <span style="color: #2a9d8f;">Izin: <span id="count-izin">0</span></span>
                    <span style="color: var(--danger);">Alpa: <span id="count-alpa">0</span></span>
                </div>
            </div>
        </div>

        <div id="absen" class="section">
            <h1>Buku Absensi Semester</h1>
            
            <div class="settings-box">
                <label style="font-weight: bold;">Total Hari Efektif 1 Semester:</label>
                <input type="number" id="total-hari-semester" class="input-small" value="100" oninput="hitungSemuaSemester()">
                <span>Hari</span>
            </div>
            
            <p style="font-size: 12px; color: #666; margin-bottom: 10px;">
                *Catatan: Nama berwarna <b style="color:red;">Merah</b> = Alpa > 5. Nama berwarna <b style="color:darkblue;">Biru Gelap</b> = Sakit > 5.
            </p>

            <div class="table-container">
                <table id="tabel-absen">
                    <thead>
                        <tr>
                            <th>No</th>
                            <th>Nama Siswa</th>
                            <th>Harian</th>
                            <th>Total Hadir</th>
                            <th>Total Sakit</th>
                            <th>Total Alpa</th>
                            <th>% Smstr</th>
                        </tr>
                    </thead>
                    <tbody id="body-absen">
                        </tbody>
                </table>
            </div>
        </div>

        <div id="kas" class="section">
            <h1>Buku Kas Semester</h1>
            
            <div class="settings-box">
                <label style="font-weight: bold;">Target Kas Per Siswa (1 Smstr): Rp</label>
                <input type="number" id="target-kas-siswa" style="width: 120px; padding: 8px;" value="150000" oninput="prosesSemuaKas()">
                <div style="margin-left: auto; font-weight: bold; color: var(--primary);">
                    Terkumpul: <span id="total-kas-rp">Rp 0</span>
                </div>
            </div>

            <div class="table-container">
                <table id="tabel-kas">
                    <thead>
                        <tr>
                            <th>No</th>
                            <th>Nama Siswa</th>
                            <th>Nominal Bayar (Rp)</th>
                            <th>Kas Harian (Auto)</th>
                        </tr>
                    </thead>
                    <tbody id="body-kas">
                        </tbody>
                </table>
            </div>
        </div>

        <div id="jurnal" class="section">
            <h1>Jurnal Kelas</h1>
            <div style="margin-bottom: 15px;">
                <label>Tanggal</label>
                <input type="date" id="jurnal-tanggal">
            </div>
            <div style="margin-bottom: 15px;">
                <label>Mata Pelajaran & Guru</label>
                <input type="text" placeholder="Contoh: Matematika - Pak Budi">
            </div>
            <button style="background: var(--primary); color: white; padding: 12px 25px; border: none; border-radius: 8px; cursor: pointer; font-weight: bold;" onclick="alert('Jurnal berhasil disimpan!')">Simpan Jurnal</button>
        </div>

    </div>

    <script>
        const siswa = [
            "Agung Ramaindra Syahputra", "Ainun Tiyas martiningsih", "Alessia Alzena Riangga",
            "Almaira Faiqha", "Alya Faliha", "Amirah Julveni Maulana", "Andi Aqil",
            "Aurora Felicia Angelie", "Bilqis Melani", "Chaya muja syatira",
            "Fadhil oktavian nabil", "fahmi firjatullah", "faiz Alfarizi",
            "farhan umaydillah radjiansyah", "fathir pradipta alfarizi", "fazril nizart",
            "januari", "jihaan kaltsum khairunnisa", "juan edra abiya",
            "Khanza Rista Bhanuwati", "Kristiana Berta", "Marselino Prasetyo wijaya",
            "M. Fadil Putra pratama", "M. Kafka adri Al-Maliq 67 sigmo", "M. Luthfi faturrahman",
            "Natalis Fedora Purba", "Rafif Rakha Arkana Dih 🥀", "Rayhan Qolbu",
            "Runika Alzariva", "Suci Pratiwi", "Syafa Damia Sakhi", "Syech Faris maulana",
            "Tegar Tri Pambudi", "Tersa Usela", "Tiara Husna Humairah", "Zhafira Mayarista"
        ];
        
        const totalSiswa = siswa.length;
        let chartAbsenInst, chartKasInst;

        // Inisialisasi Chart.js
        window.onload = function() {
            // Set Tanggal Jurnal
            document.getElementById('jurnal-tanggal').valueAsDate = new Date();

            // Chart Kehadiran
            const ctxAbsen = document.getElementById('absenChart').getContext('2d');
            chartAbsenInst = new Chart(ctxAbsen, {
                type: 'doughnut',
                data: {
                    labels: ['Hadir', 'Tidak Hadir'],
                    datasets: [{
                        data: [0, 100],
                        backgroundColor: ['#2a9d8f', '#e63946'],
                        borderWidth: 0
                    }]
                },
                options: { responsive: true, maintainAspectRatio: true, plugins: { legend: { position: 'bottom' } } }
            });

            // Chart Kas
            const ctxKas = document.getElementById('kasChart').getContext('2d');
            chartKasInst = new Chart(ctxKas, {
                type: 'doughnut',
                data: {
                    labels: ['Terkumpul', 'Kekurangan'],
                    datasets: [{
                        data: [0, 100],
                        backgroundColor: ['#0077b6', '#caf0f8'],
                        borderWidth: 0
                    }]
                },
                options: { responsive: true, maintainAspectRatio: true, plugins: { legend: { position: 'bottom' } } }
            });

            RenderTabel();
        };

        function showSection(sectionId) {
            document.querySelectorAll('.section').forEach(sec => sec.classList.remove('active'));
            document.querySelectorAll('.nav-btn').forEach(btn => btn.classList.remove('active'));
            document.getElementById(sectionId).classList.add('active');
            event.currentTarget.classList.add('active');
        }

        // Render Tabel Absen & Kas
        function RenderTabel() {
            const bodyAbsen = document.getElementById('body-absen');
            const bodyKas = document.getElementById('body-kas');

            siswa.forEach((nama, index) => {
                // Tabel Absen
                let trA = document.createElement('tr');
                trA.innerHTML = `
                    <td>${index + 1}</td>
                    <td id="nama_siswa_${index}" style="font-weight: 500;">${nama}</td>
                    <td>
                        <div class="radio-group">
                            <label><input type="radio" name="absen_hari_${index}" value="H" onchange="hitungHariIni()">H</label>
                            <label><input type="radio" name="absen_hari_${index}" value="S" onchange="hitungHariIni()">S</label>
                            <label><input type="radio" name="absen_hari_${index}" value="I" onchange="hitungHariIni()">I</label>
                            <label><input type="radio" name="absen_hari_${index}" value="A" onchange="hitungHariIni()">A</label>
                        </div>
                    </td>
                    <td><input type="number" class="input-small" id="hadir_smstr_${index}" value="0" oninput="hitungSemuaSemester()"></td>
                    <td><input type="number" class="input-small" id="sakit_smstr_${index}" value="0" oninput="hitungSemuaSemester()"></td>
                    <td><input type="number" class="input-small" id="alpa_smstr_${index}" value="0" oninput="hitungSemuaSemester()"></td>
                    <td><span id="persen_smstr_${index}" style="font-weight:bold;">0%</span></td>
                `;
                bodyAbsen.appendChild(trA);

                // Tabel Kas
                let checkboxes = '';
                for(let i=1; i<=31; i++) {
                    checkboxes += `<input type="checkbox" id="kas_harian_${index}_${i}" disabled style="width: 15px; height: 15px;">`;
                }

                let trK = document.createElement('tr');
                trK.innerHTML = `
                    <td>${index + 1}</td>
                    <td>${nama}</td>
                    <td><input type="number" id="kas_bulanan_${index}" style="width: 100px; padding: 8px;" value="0" oninput="prosesSemuaKas()"></td>
                    <td><div style="display: flex; gap: 3px; flex-wrap: wrap;">${checkboxes}</div></td>
                `;
                bodyKas.appendChild(trK);
            });
        }

        // LOGIKA ABSENSI
        function hitungHariIni() {
            let h = 0, s = 0, i = 0, a = 0;
            for(let x=0; x<totalSiswa; x++) {
                let radio = document.querySelector(`input[name="absen_hari_${x}"]:checked`);
                if(radio) {
                    if(radio.value === "H") h++;
                    if(radio.value === "S") s++;
                    if(radio.value === "I") i++;
                    if(radio.value === "A") a++;
                }
            }
            document.getElementById('count-hadir').innerText = h;
            document.getElementById('count-sakit').innerText = s;
            document.getElementById('count-izin').innerText = i;
            document.getElementById('count-alpa').innerText = a;
        }

        function hitungSemuaSemester() {
            let totalHariSmstr = parseInt(document.getElementById('total-hari-semester').value) || 1;
            let totalHadirKelas = 0;

            for(let i=0; i<totalSiswa; i++) {
                let hadir = parseInt(document.getElementById(`hadir_smstr_${i}`).value) || 0;
                let sakit = parseInt(document.getElementById(`sakit_smstr_${i}`).value) || 0;
                let alpa = parseInt(document.getElementById(`alpa_smstr_${i}`).value) || 0;

                let persenIndividu = (hadir / totalHariSmstr) * 100;
                if(persenIndividu > 100) persenIndividu = 100;
                
                let namaCell = document.getElementById(`nama_siswa_${i}`);
                let persenCell = document.getElementById(`persen_smstr_${i}`);
                
                persenCell.innerText = persenIndividu.toFixed(1) + '%';
                totalHadirKelas += hadir;

                // Logika Warna Nama Siswa
                if (alpa > 5) {
                    namaCell.style.color = "var(--danger)"; // Merah
                } else if (sakit > 5) {
                    namaCell.style.color = "var(--warning)"; // Biru Gelap
                } else {
                    namaCell.style.color = "var(--text-dark)"; // Normal
                }
            }

            // Update Chart Absen di Dashboard
            let rataRataKelas = (totalHadirKelas / (totalHariSmstr * totalSiswa)) * 100;
            if(rataRataKelas > 100) rataRataKelas = 100;

            document.getElementById('teks-persen-absen').innerText = rataRataKelas.toFixed(1) + '%';
            chartAbsenInst.data.datasets[0].data = [rataRataKelas, 100 - rataRataKelas];
            chartAbsenInst.update();
        }

        // LOGIKA KAS
        function prosesSemuaKas() {
            let targetPerSiswa = parseInt(document.getElementById('target-kas-siswa').value) || 1;
            let targetTotalKelas = targetPerSiswa * totalSiswa;
            let totalTerkumpul = 0;

            for(let i=0; i<totalSiswa; i++) {
                let nominal = parseInt(document.getElementById(`kas_bulanan_${i}`).value) || 0;
                totalTerkumpul += nominal;

                // Auto Centang Harian (dibagi 1000)
                let hariCek = Math.floor(nominal / 1000);
                if(hariCek > 31) hariCek = 31;
                for(let d=1; d<=31; d++) {
                    document.getElementById(`kas_harian_${i}_${d}`).checked = (d <= hariCek);
                }
            }

            // Update Teks Rupiah
            document.getElementById('total-kas-rp').innerText = new Intl.NumberFormat('id-ID', {
                style: 'currency', currency: 'IDR', minimumFractionDigits: 0
            }).format(totalTerkumpul);

            // Update Chart Kas di Dashboard
            let persenKas = (totalTerkumpul / targetTotalKelas) * 100;
            if(persenKas > 100) persenKas = 100;

            document.getElementById('teks-persen-kas').innerText = persenKas.toFixed(1) + '%';
            chartKasInst.data.datasets[0].data = [persenKas, 100 - persenKas];
            chartKasInst.update();
        }
    </script>
</body>
</html>

                let nominal = parseInt(document.getElementById(`kas_bulanan_${i}`).value) || 0;
                totalTerkumpul += nominal;

                // Auto Centang Harian (dibagi 1000)
                let hariCek = Math.floor(nominal / 1000);
                if(hariCek > 31) hariCek = 31;
                for(let d=1; d<=31; d++) {
                    document.getElementById(`kas_harian_${i}_${d}`).checked = (d <= hariCek);
                }
            }

            // Update Teks Rupiah
            document.getElementById('total-kas-rp').innerText = new Intl.NumberFormat('id-ID', {
                style: 'currency', currency: 'IDR', minimumFractionDigits: 0
            }).format(totalTerkumpul);

            // Update Chart Kas di Dashboard
            let persenKas = (totalTerkumpul / targetTotalKelas) * 100;
            if(persenKas > 100) persenKas = 100;

            document.getElementById('teks-persen-kas').innerText = persenKas.toFixed(1) + '%';
            chartKasInst.data.datasets[0].data = [persenKas, 100 - persenKas];
            chartKasInst.update();
        }
    </script>
</body>
</html>

