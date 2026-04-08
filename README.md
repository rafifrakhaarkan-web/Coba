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
            --warning: #00008b; 
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Poppins', sans-serif; }

        body {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 50%, var(--accent) 100%);
            background-attachment: fixed; color: var(--text-dark); min-height: 100vh; display: flex;
        }

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

        .main-content { flex: 1; padding: 2rem; overflow-y: auto; height: 100vh; }

        .section {
            display: none; background: var(--white); backdrop-filter: blur(10px); border-radius: 20px;
            padding: 2rem; box-shadow: 0 10px 30px rgba(0,0,0,0.1); animation: fadeIn 0.4s ease-in-out;
        }
        .section.active { display: block; }

        h1 { color: var(--primary); margin-bottom: 1.5rem; border-bottom: 2px solid var(--accent); padding-bottom: 10px; }

        .charts-container { display: flex; gap: 20px; margin-bottom: 2rem; flex-wrap: wrap; }
        
        .chart-box {
            flex: 1; min-width: 250px; background: white; padding: 20px; border-radius: 15px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05); border-top: 5px solid var(--secondary);
            display: flex; flex-direction: column; align-items: center; justify-content: center;
        }

        .chart-box h3 { color: var(--text-dark); margin-bottom: 10px; font-size: 16px; text-align: center;}
        .chart-box .persen-teks { font-size: 28px; font-weight: bold; color: var(--primary); margin-top: 10px; }
        
        .canvas-wrapper { width: 180px; height: 180px; } 

        .table-container { overflow-x: auto; }
        table { width: 100%; border-collapse: collapse; margin-top: 1rem; background: white; border-radius: 10px; overflow: hidden; font-size: 14px; }
        th, td { padding: 10px 12px; text-align: left; border-bottom: 1px solid #eee; vertical-align: middle; }
        th { background-color: var(--primary); color: white; font-weight: 500; }

        input[type="text"], input[type="number"], input[type="date"] {
            width: 100%; padding: 10px; border: 1px solid #ccc; border-radius: 8px;
        }
        .input-small { width: 60px !important; padding: 8px !important; text-align: center;}
        
        .radio-group { display: flex; gap: 8px; flex-wrap: wrap; }
        .radio-group label { cursor: pointer; display: flex; align-items: center; gap: 3px; font-size: 13px; }

        .settings-box {
            background: rgba(0, 180, 216, 0.1); padding: 15px; border-radius: 10px; margin-bottom: 15px;
            display: flex; align-items: center; gap: 10px; border: 1px dashed var(--primary); flex-wrap: wrap;
        }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

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
        <button class="nav-btn active" onclick="showSection('dashboard', this)">🏠 Dashboard</button>
        <button class="nav-btn" onclick="showSection('absen', this)">📝 Absen</button>
        <button class="nav-btn" onclick="showSection('kas', this)">💰 Kas</button>
        <button class="nav-btn" onclick="showSection('jurnal', this)">📖 Jurnal</button>
    </div>

    <div class="main-content">
        <div id="dashboard" class="section active">
            <h1>Dashboard Analytics</h1>
            <div class="charts-container">
                <div class="chart-box">
                    <h3>Kehadiran Kelas (1 Semester)</h3>
                    <div class="canvas-wrapper"><canvas id="absenChart"></canvas></div>
                    <div class="persen-teks" id="teks-persen-absen">0%</div>
                </div>
                <div class="chart-box">
                    <h3>Pencapaian Kas (1 Semester)</h3>
                    <div class="canvas-wrapper"><canvas id="kasChart"></canvas></div>
                    <div class="persen-teks" id="teks-persen-kas">0%</div>
                </div>
            </div>
            <div style="background: white; padding: 15px; border-radius: 10px; margin-bottom: 20px;">
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
                <label>Total Hari Efektif:</label>
                <input type="number" id="total-hari-semester" class="input-small" value="100" oninput="hitungSemuaSemester()">
            </div>
            <div class="table-container">
                <table>
                    <thead>
                        <tr>
                            <th>No</th><th>Nama Siswa</th><th>Harian</th><th>Hadir</th><th>Sakit</th><th>Alpa</th><th>%</th>
                        </tr>
                    </thead>
                    <tbody id="body-absen"></tbody>
                </table>
            </div>
        </div>

        <div id="kas" class="section">
            <h1>Buku Kas Semester</h1>
            <div class="settings-box">
                <label>Target Kas (Rp):</label>
                <input type="number" id="target-kas-siswa" style="width: 120px;" value="150000" oninput="prosesSemuaKas()">
                <span id="total-kas-rp" style="margin-left: auto; font-weight: bold;">Rp 0</span>
            </div>
            <div class="table-container">
                <table>
                    <thead>
                        <tr><th>No</th><th>Nama Siswa</th><th>Nominal (Rp)</th><th>Harian (Auto)</th></tr>
                    </thead>
                    <tbody id="body-kas"></tbody>
                </table>
            </div>
        </div>

        <div id="jurnal" class="section">
            <h1>Jurnal Kelas</h1>
            <input type="date" id="jurnal-tanggal" style="margin-bottom: 10px;">
            <input type="text" placeholder="Mapel & Guru" style="margin-bottom: 10px;">
            <button onclick="alert('Simpan!')" style="padding: 10px; background: var(--primary); color: white; border: none; border-radius: 5px;">Simpan</button>
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
        
        let chartAbsenInst, chartKasInst;

        window.onload = function() {
            document.getElementById('jurnal-tanggal').valueAsDate = new Date();
            initCharts();
            RenderTabel();
        };

        function initCharts() {
            const ctxAbsen = document.getElementById('absenChart').getContext('2d');
            chartAbsenInst = new Chart(ctxAbsen, {
                type: 'doughnut',
                data: { labels: ['Hadir', 'Tidak'], datasets: [{ data: [0, 100], backgroundColor: ['#2a9d8f', '#e63946'] }] },
                options: { responsive: true }
            });

            const ctxKas = document.getElementById('kasChart').getContext('2d');
            chartKasInst = new Chart(ctxKas, {
                type: 'doughnut',
                data: { labels: ['Terkumpul', 'Sisa'], datasets: [{ data: [0, 100], backgroundColor: ['#0077b6', '#caf0f8'] }] },
                options: { responsive: true }
            });
        }

        function showSection(sectionId, btn) {
            document.querySelectorAll('.section').forEach(sec => sec.classList.remove('active'));
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            document.getElementById(sectionId).classList.add('active');
            btn.classList.add('active');
        }

        function RenderTabel() {
            const bodyAbsen = document.getElementById('body-absen');
            const bodyKas = document.getElementById('body-kas');
            
            // Kosongkan tabel dulu supaya tidak double saat dipanggil
            bodyAbsen.innerHTML = "";
            bodyKas.innerHTML = "";

            siswa.forEach((nama, index) => {
                let trA = `<tr>
                    <td>${index + 1}</td>
                    <td id="nama_siswa_${index}">${nama}</td>
                    <td><div class="radio-group">
                        <label><input type="radio" name="abs_${index}" value="H" onchange="hitungHariIni()">H</label>
                        <label><input type="radio" name="abs_${index}" value="S" onchange="hitungHariIni()">S</label>
                        <label><input type="radio" name="abs_${index}" value="A" onchange="hitungHariIni()">A</label>
                    </div></td>
                    <td><input type="number" class="input-small" id="hadir_smstr_${index}" value="0" oninput="hitungSemuaSemester()"></td>
                    <td><input type="number" class="input-small" id="sakit_smstr_${index}" value="0" oninput="hitungSemuaSemester()"></td>
                    <td><input type="number" class="input-small" id="alpa_smstr_${index}" value="0" oninput="hitungSemuaSemester()"></td>
                    <td><span id="persen_smstr_${index}">0%</span></td>
                </tr>`;
                bodyAbsen.innerHTML += trA;

                let trK = `<tr>
                    <td>${index + 1}</td><td>${nama}</td>
                    <td><input type="number" id="kas_bulanan_${index}" value="0" oninput="prosesSemuaKas()" style="width:80px"></td>
                    <td><div id="box_kas_${index}" style="display:flex; gap:2px; flex-wrap:wrap"></div></td>
                </tr>`;
                bodyKas.innerHTML += trK;
                
                // Isi kotak kas kecil
                const box = document.getElementById(`box_kas_${index}`);
                for(let i=1; i<=31; i++) {
                    box.innerHTML += `<div id="k_${index}_${i}" style="width:8px; height:8px; background:#eee; border-radius:2px"></div>`;
                }
            });
        }

        function hitungHariIni() {
            let h=0, s=0, i=0, a=0;
            siswa.forEach((_, x) => {
                let r = document.querySelector(`input[name="abs_${x}"]:checked`);
                if(r) {
                    if(r.value==="H") h++;
                    if(r.value==="S") s++;
                    if(r.value==="A") a++;
                }
            });
            document.getElementById('count-hadir').innerText = h;
            document.getElementById('count-sakit').innerText = s;
            document.getElementById('count-alpa').innerText = a;
        }

        function hitungSemuaSemester() {
            let totalHari = parseInt(document.getElementById('total-hari-semester').value) || 1;
            let totalHadirKelas = 0;
            siswa.forEach((_, i) => {
                let hadir = parseInt(document.getElementById(`hadir_smstr_${i}`).value) || 0;
                let alpa = parseInt(document.getElementById(`alpa_smstr_${i}`).value) || 0;
                let persen = (hadir / totalHari) * 100;
                document.getElementById(`persen_smstr_${i}`).innerText = Math.min(persen, 100).toFixed(0) + '%';
                document.getElementById(`nama_siswa_${i}`).style.color = alpa > 5 ? 'red' : '#023e8a';
                totalHadirKelas += hadir;
            });
            let rata = (totalHadirKelas / (totalHari * siswa.length)) * 100;
            document.getElementById('teks-persen-absen').innerText = Math.min(rata, 100).toFixed(1) + '%';
            chartAbsenInst.data.datasets[0].data = [rata, 100 - rata];
            chartAbsenInst.update();
        }

        function prosesSemuaKas() {
            let target = parseInt(document.getElementById('target-kas-siswa').value) || 1;
            let total = 0;
            siswa.forEach((_, i) => {
                let n = parseInt(document.getElementById(`kas_bulanan_${i}`).value) || 0;
                total += n;
                let cek = Math.floor(n / 1000);
                for(let d=1; d<=31; d++) {
                    document.getElementById(`k_${i}_${d}`).style.background = d <= cek ? '#0077b6' : '#eee';
                }
            });
            document.getElementById('total-kas-rp').innerText = "Rp " + total.toLocaleString();
            let p = (total / (target * siswa.length)) * 100;
            document.getElementById('teks-persen-kas').innerText = Math.min(p, 100).toFixed(1) + '%';
            chartKasInst.data.datasets[0].data = [p, 100 - p];
            chartKasInst.update();
        }
    </script>
</body>
</html>
