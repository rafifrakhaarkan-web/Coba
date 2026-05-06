<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Kelas 9D - Liquid Glass UI</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }

        :root {
            --glass-white: rgba(255, 255, 255, 0.1);
            --glass-border: rgba(255, 255, 255, 0.2);
            --glass-strong: rgba(255, 255, 255, 0.15);
            --text-primary: rgba(255, 255, 255, 0.95);
            --text-secondary: rgba(255, 255, 255, 0.7);
            --primary: #007AFF;
            --success: #34C759;
            --warning: #FF9500;
            --danger: #FF3B30;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', 'SF Pro Text', sans-serif;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
            min-height: 100vh;
            padding: 20px;
            color: var(--text-primary);
            position: relative;
            overflow-x: hidden;
        }

        body::before {
            content: '';
            position: fixed;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(0, 122, 255, 0.1) 0%, transparent 70%);
            animation: pulse 15s ease-in-out infinite;
            pointer-events: none;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1) translate(0, 0); opacity: 0.5; }
            50% { transform: scale(1.1) translate(5%, 5%); opacity: 0.8; }
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.05);
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.2);
            border-radius: 10px;
        }

        .container {
            max-width: 1000px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
        }

        .header-title {
            text-align: center;
            margin-bottom: 30px;
            font-size: 32px;
            font-weight: 800;
            letter-spacing: 1px;
            text-transform: uppercase;
            background: linear-gradient(to right, #fff, #a5d8ff);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 4px 20px rgba(0, 122, 255, 0.4);
        }

        /* Navigation Grid */
        .glass-card {
            background: var(--glass-white);
            backdrop-filter: saturate(180%) blur(20px);
            -webkit-backdrop-filter: saturate(180%) blur(20px);
            border: 1px solid var(--glass-border);
            border-radius: 24px;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.2);
        }

        .glass-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 1px;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
        }

        .glass-card:hover {
            background: var(--glass-strong);
            border-color: rgba(255, 255, 255, 0.3);
            transform: translateY(-2px);
            box-shadow: 0 12px 40px 0 rgba(0, 122, 255, 0.2);
        }

        .nav-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 16px;
            margin-bottom: 20px;
        }

        .nav-item {
            aspect-ratio: 16/10;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 12px;
            cursor: pointer;
            padding: 15px;
        }

        .nav-item.active {
            background: linear-gradient(135deg, rgba(0, 122, 255, 0.3) 0%, rgba(255, 255, 255, 0.1) 100%);
            border-color: rgba(255, 255, 255, 0.5);
            box-shadow: 0 0 20px rgba(0, 122, 255, 0.3);
        }

        .nav-item .icon {
            width: 32px;
            height: 32px;
            fill: var(--text-secondary);
            transition: all 0.3s;
        }

        .nav-item.active .icon {
            fill: var(--text-primary);
            filter: drop-shadow(0 0 8px rgba(255, 255, 255, 0.5));
        }

        .nav-item .label {
            font-size: 14px;
            font-weight: 600;
            letter-spacing: 0.5px;
            color: var(--text-secondary);
        }

        .nav-item.active .label {
            color: var(--text-primary);
        }

        /* Content Area */
        .content-large {
            min-height: 500px;
            padding: 32px;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
            margin-bottom: 24px;
        }

        .stat-item {
            background: var(--glass-white);
            backdrop-filter: saturate(180%) blur(20px);
            -webkit-backdrop-filter: saturate(180%) blur(20px);
            border: 1px solid var(--glass-border);
            border-radius: 16px;
            padding: 24px;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            text-align: center;
        }

        .stat-item:hover {
            background: var(--glass-strong);
            transform: translateY(-5px);
            border-color: rgba(255, 255, 255, 0.4);
        }

        .stat-item h3 {
            font-size: 14px;
            font-weight: 600;
            color: var(--text-secondary);
            margin-bottom: 12px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .stat-item .number {
            font-size: 42px;
            font-weight: 800;
            letter-spacing: -1.5px;
            margin-bottom: 8px;
            color: var(--text-primary);
            text-shadow: 0 2px 10px rgba(0,0,0,0.2);
        }

        .stat-item .label {
            font-size: 13px;
            color: var(--text-secondary);
            font-weight: 500;
        }

        /* Table Styles */
        .table-container {
            background: var(--glass-white);
            border: 1px solid var(--glass-border);
            border-radius: 20px;
            overflow-x: auto;
            margin-top: 20px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            min-width: 600px;
        }

        th {
            background: rgba(0, 0, 0, 0.2);
            padding: 16px 20px;
            text-align: left;
            font-size: 13px;
            font-weight: 600;
            color: #a5d8ff;
            text-transform: uppercase;
            letter-spacing: 1px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        td {
            padding: 16px 20px;
            font-size: 15px;
            font-weight: 500;
            border-bottom: 1px solid rgba(255, 255, 255, 0.05);
        }

        tr:hover td {
            background: rgba(255, 255, 255, 0.05);
        }

        /* Page Content Animation */
        .page {
            display: none;
            animation: fadeIn 0.5s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .page.active {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(15px) scale(0.98); }
            to { opacity: 1; transform: translateY(0) scale(1); }
        }

        .page-title {
            font-size: 32px;
            font-weight: 800;
            letter-spacing: -0.6px;
            margin-bottom: 24px;
            border-bottom: 2px solid var(--glass-border);
            padding-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        /* Form Elements */
        .form-group {
            margin-bottom: 20px;
        }

        .form-group label {
            display: block;
            font-size: 14px;
            font-weight: 600;
            color: #a5d8ff;
            margin-bottom: 8px;
            letter-spacing: 0.5px;
        }

        input[type="text"], input[type="number"], input[type="date"], textarea, select {
            width: 100%;
            padding: 16px;
            background: rgba(0, 0, 0, 0.2);
            border: 1px solid var(--glass-border);
            border-radius: 14px;
            color: var(--text-primary);
            font-family: inherit;
            font-size: 15px;
            transition: all 0.3s;
        }

        input:focus, textarea:focus, select:focus {
            outline: none;
            background: rgba(0, 0, 0, 0.4);
            border-color: var(--primary);
            box-shadow: 0 0 0 4px rgba(0, 122, 255, 0.2);
        }

        /* Buttons */
        .btn {
            background: linear-gradient(135deg, var(--primary) 0%, #0056b3 100%);
            border: none;
            color: white;
            padding: 16px 32px;
            border-radius: 14px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 4px 15px rgba(0, 122, 255, 0.3);
            width: 100%;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 122, 255, 0.5);
        }

        /* Badges */
        .badge {
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 700;
            text-transform: uppercase;
        }
        .badge-hadir { background: rgba(52, 199, 89, 0.2); color: var(--success); border: 1px solid var(--success); }
        .badge-sakit { background: rgba(255, 149, 0, 0.2); color: var(--warning); border: 1px solid var(--warning); }
        .badge-izin { background: rgba(0, 122, 255, 0.2); color: var(--primary); border: 1px solid var(--primary); }

        /* Announcement Card */
        .announcement-card {
            background: linear-gradient(135deg, rgba(0, 122, 255, 0.15) 0%, rgba(90, 200, 250, 0.1) 100%);
            border: 1px solid rgba(0, 122, 255, 0.3);
            padding: 24px;
            border-radius: 20px;
            margin-top: 20px;
            position: relative;
            overflow: hidden;
        }
        
        .announcement-card::before {
            content: '📌';
            position: absolute;
            right: 20px;
            top: 20px;
            font-size: 40px;
            opacity: 0.2;
        }

        /* Interactive Bubbles Background */
        .bubble {
            position: fixed;
            background: radial-gradient(circle at 30% 30%, rgba(255, 255, 255, 0.4), rgba(255, 255, 255, 0.05));
            border-radius: 50%;
            pointer-events: none;
            z-index: 0;
            animation: floatUp 8s ease-in forwards;
        }

        @keyframes floatUp {
            0% { transform: translateY(100vh) scale(0); opacity: 0; }
            20% { opacity: 1; }
            100% { transform: translateY(-20vh) scale(1.5); opacity: 0; }
        }

        @media (max-width: 768px) {
            .nav-grid { grid-template-columns: repeat(2, 1fr); }
            .stats-grid { grid-template-columns: 1fr; }
            .content-large { padding: 20px; }
        }
    </style>
</head>
<body>
    <div id="bubble-container"></div>

    <div class="container">
        <h1 class="header-title">Portal Kelas 9D</h1>

        <div class="nav-grid">
            <div class="glass-card nav-item active" onclick="showPage('dashboard', this)">
                <svg class="icon" viewBox="0 0 24 24"><path d="M3 13h8V3H3v10zm0 8h8v-6H3v6zm10 0h8V11h-8v10zm0-18v6h8V3h-8z"/></svg>
                <div class="label">Dashboard</div>
            </div>
            <div class="glass-card nav-item" onclick="showPage('absen', this)">
                <svg class="icon" viewBox="0 0 24 24"><path d="M16 11c1.66 0 2.99-1.34 2.99-3S17.66 5 16 5c-1.66 0-3 1.34-3 3s1.34 3 3 3zm-8 0c1.66 0 2.99-1.34 2.99-3S9.66 5 8 5C6.34 5 5 6.34 5 8s1.34 3 3 3zm0 2c-2.33 0-7 1.17-7 3.5V19h14v-2.5c0-2.33-4.67-3.5-7-3.5zm8 0c-.29 0-.62.02-.97.05 1.16.84 1.97 1.97 1.97 3.45V19h6v-2.5c0-2.33-4.67-3.5-7-3.5z"/></svg>
                <div class="label">Absensi</div>
            </div>
            <div class="glass-card nav-item" onclick="showPage('jurnal', this)">
                <svg class="icon" viewBox="0 0 24 24"><path d="M18 2H6c-1.1 0-2 .9-2 2v16c0 1.1.9 2 2 2h12c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2zM6 4h5v8l-2.5-1.5L6 12V4z"/></svg>
                <div class="label">Jurnal</div>
            </div>
            <div class="glass-card nav-item" onclick="showPage('kas', this)">
                <svg class="icon" viewBox="0 0 24 24"><path d="M21 18v1c0 1.1-.9 2-2 2H5c-1.11 0-2-.9-2-2V5c0-1.1.89-2 2-2h14c1.1 0 2 .9 2 2v1h-9c-1.11 0-2 .9-2 2v8c0 1.1.89 2 2 2h9zm-9-2h10V8H12v8zm4-2.5c-.83 0-1.5-.67-1.5-1.5s.67-1.5 1.5-1.5 1.5.67 1.5 1.5-.67 1.5-1.5 1.5z"/></svg>
                <div class="label">Kas Kelas</div>
            </div>
        </div>

        <div class="glass-card content-large">
            
            <div id="dashboard" class="page active">
                <h2 class="page-title">Ringkasan Hari Ini</h2>
                
                <div class="stats-grid">
                    <div class="stat-item">
                        <h3>Total Siswa</h3>
                        <div class="number">36</div>
                        <div class="label">Terdaftar</div>
                    </div>
                    <div class="stat-item">
                        <h3>Tingkat Kehadiran</h3>
                        <div class="number" style="color: var(--success);">95%</div>
                        <div class="label">Hari Ini</div>
                    </div>
                    <div class="stat-item">
                        <h3>Saldo Kas</h3>
                        <div class="number" style="font-size: 32px; color: #a5d8ff;">Rp 450k</div>
                        <div class="label">Terkumpul</div>
                    </div>
                </div>

                <div class="announcement-card">
                    <h2 style="font-size: 20px; font-weight: 700; margin-bottom: 12px; color: #a5d8ff;">Pengumuman Kelas</h2>
                    <p style="color: var(--text-secondary); line-height: 1.6;">
                        <strong>Jumat, 10 Mei:</strong> Akan diadakan kerja bakti membersihkan ruang kelas. Diharapkan semua siswa membawa alat kebersihan masing-masing. Jangan lupa bayar uang kas minggu ini kepada bendahara!
                    </p>
                </div>
            </div>

            <div id="absen" class="page">
                <h2 class="page-title">Absensi Harian</h2>
                <div class="form-group" style="max-width: 300px;">
                    <label>Pilih Tanggal</label>
                    <input type="date" value="2026-05-06">
                </div>
                
                <div class="table-container">
                    <table>
                        <thead>
                            <tr>
                                <th>No</th>
                                <th>Nama Siswa</th>
                                <th>Status</th>
                                <th>Keterangan</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>1</td>
                                <td>Ahmad Budi</td>
                                <td><span class="badge badge-hadir">Hadir</span></td>
                                <td>-</td>
                            </tr>
                            <tr>
                                <td>2</td>
                                <td>Citra Dewi</td>
                                <td><span class="badge badge-sakit">Sakit</span></td>
                                <td>Surat dokter (Demam)</td>
                            </tr>
                            <tr>
                                <td>3</td>
                                <td>Dimas Anggara</td>
                                <td><span class="badge badge-izin">Izin</span></td>
                                <td>Acara Keluarga</td>
                            </tr>
                            <tr>
                                <td>4</td>
                                <td>Eka Putra</td>
                                <td><span class="badge badge-hadir">Hadir</span></td>
                                <td>-</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>

            <div id="jurnal" class="page">
                <h2 class="page-title">Jurnal Kelas</h2>
                
                <form onsubmit="event.preventDefault(); alert('Jurnal berhasil disimpan!');">
                    <div class="stats-grid" style="grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 0;">
                        <div class="form-group">
                            <label>Mata Pelajaran</label>
                            <input type="text" placeholder="Cth: Matematika" required>
                        </div>
                        <div class="form-group">
                            <label>Nama Guru</label>
                            <input type="text" placeholder="Cth: Bpk. Supriyanto" required>
                        </div>
                    </div>
                    <div class="form-group">
                        <label>Materi yang diajarkan</label>
                        <textarea placeholder="Tulis ringkasan materi di sini..." required></textarea>
                    </div>
                    <button type="submit" class="btn">Simpan Jurnal</button>
                </form>

                <h3 style="margin-top: 40px; margin-bottom: 15px; color: #a5d8ff;">Catatan Terakhir</h3>
                <div class="announcement-card" style="margin-top: 0; background: rgba(255,255,255,0.05); border-color: rgba(255,255,255,0.1);">
                    <div style="font-size: 12px; color: var(--primary); margin-bottom: 5px;">06 Mei 2026 • Bahasa Indonesia</div>
                    <strong style="font-size: 18px;">Teks Cerpen</strong>
                    <p style="margin-top: 8px; color: var(--text-secondary);">Membahas unsur intrinsik dan ekstrinsik dari sebuah cerita pendek. Tugas: Membuat cerpen tema bebas dikumpulkan minggu depan.</p>
                </div>
            </div>

            <div id="kas" class="page">
                <h2 class="page-title">Keuangan Kelas</h2>
                
                <div class="stats-grid" style="grid-template-columns: 1fr 1fr;">
                    <div class="stat-item" style="background: rgba(52, 199, 89, 0.1); border-color: rgba(52, 199, 89, 0.3);">
                        <h3>Pemasukan</h3>
                        <div class="number" style="color: var(--success); font-size: 32px;">+ Rp 500.000</div>
                    </div>
                    <div class="stat-item" style="background: rgba(255, 59, 48, 0.1); border-color: rgba(255, 59, 48, 0.3);">
                        <h3>Pengeluaran</h3>
                        <div class="number" style="color: var(--danger); font-size: 32px;">- Rp 50.000</div>
                    </div>
                </div>

                <div class="table-container">
                    <table>
                        <thead>
                            <tr>
                                <th>Tanggal</th>
                                <th>Keterangan</th>
                                <th>Nominal</th>
                                <th>Tipe</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>01 Mei 2026</td>
                                <td>Iuran Kas Minggu ke-1</td>
                                <td style="color: var(--success);">Rp 100.000</td>
                                <td><span class="badge badge-hadir">Masuk</span></td>
                            </tr>
                            <tr>
                                <td>03 Mei 2026</td>
                                <td>Beli Spidol & Penghapus Papan</td>
                                <td style="color: var(--danger);">Rp 50.000</td>
                                <td><span class="badge badge-sakit" style="color:var(--danger); border-color:var(--danger);">Keluar</span></td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>

        </div>
    </div>

    <script>
        // Fungsi untuk mengganti halaman
        function showPage(pageId, element) {
            // Sembunyikan semua halaman
            const pages = document.querySelectorAll('.page');
            pages.forEach(page => page.classList.remove('active'));

            // Tampilkan halaman yang dipilih
            document.getElementById(pageId).classList.add('active');

            // Update state tombol navigasi
            const navItems = document.querySelectorAll('.nav-item');
            navItems.forEach(item => item.classList.remove('active'));
            element.classList.add('active');

            // Buat efek gelembung setiap kali pindah tab
            createBubble();
        }

        // Efek background bubble interaktif (Liquid vibe)
        function createBubble() {
            const container = document.getElementById('bubble-container');
            const bubble = document.createElement('div');
            bubble.classList.add('bubble');
            
            // Random ukuran dan posisi
            const size = Math.random() * 60 + 20; // 20px - 80px
            const left = Math.random() * 100;
            const duration = Math.random() * 3 + 4; // 4s - 7s

            bubble.style.width = `${size}px`;
            bubble.style.height = `${size}px`;
            bubble.style.left = `${left}vw`;
            bubble.style.animationDuration = `${duration}s`;

            container.appendChild(bubble);

            // Hapus elemen setelah animasi selesai agar tidak membebani memori
            setTimeout(() => {
                bubble.remove();
            }, duration * 1000);
        }

        // Jalankan beberapa bubble secara otomatis saat pertama kali load
        setInterval(createBubble, 2000);
    </script>
</body>
</html>
