<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rental TEGUH-trans</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --theme-r: 37;
            --theme-g: 99;
            --theme-b: 235;
        }
        .bg-theme { background-color: rgb(var(--theme-r), var(--theme-g), var(--theme-b)); }
        .text-theme { color: rgb(var(--theme-r), var(--theme-g), var(--theme-b)); }
        .border-theme { border-color: rgb(var(--theme-r), var(--theme-g), var(--theme-b)); }
        .ring-theme:focus { --tw-ring-color: rgb(var(--theme-r), var(--theme-g), var(--theme-b)); }
    </style>
</head>
<body class="bg-gray-50 text-gray-800 font-sans antialiased">

    <!-- HALAMAN LOGIN -->
    <div id="loginPage" class="fixed inset-0 bg-gray-900 z-50 flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl shadow-xl w-full max-w-md p-8">
            <div class="text-center mb-8">
                <h1 id="loginBrandTitle" class="text-3xl font-black text-theme">TEGUH-trans</h1>
                <p class="text-gray-500 text-sm mt-1">Silakan masuk ke panel admin rental</p>
            </div>
            <form onsubmit="handleLogin(event)" class="space-y-4">
                <div>
                    <label class="block text-sm font-medium mb-1">Username</label>
                    <input type="text" id="loginUser" required class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 border-theme ring-theme">
                </div>
                <div>
                    <label class="block text-sm font-medium mb-1">Kata Sandi</label>
                    <input type="password" id="loginPass" required class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 border-theme ring-theme">
                </div>
                <button type="submit" class="w-full py-3 bg-theme text-white font-bold rounded-lg shadow-md transition hover:opacity-90">Masuk Sistem</button>
            </form>
            <div class="mt-6 text-center text-xs text-gray-400">
                Default Login: Username: <b>admin</b> | Sandi: <b>123</b> (Dapat diubah di pengaturan)
            </div>
        </div>
    </div>

    <!-- LAYOUT UTAMA APLIKASI -->
    <div id="appLayout" class="flex h-screen overflow-hidden hidden">
        <!-- SIDEBAR -->
        <aside class="w-64 bg-white border-r flex flex-col justify-between shrink-0">
            <div>
                <div class="p-6 border-b">
                    <h2 id="sidebarBrandName" class="text-xl font-black text-theme truncate">TEGUH-trans</h2>
                    <p class="text-xs text-gray-400 mt-0.5">Sistem Manajemen Rental</p>
                </div>
                <nav class="p-4 space-y-1">
                    <button onclick="switchTab('dashboard')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-lg font-medium text-gray-600 hover:bg-gray-100 transition active-tab">
                        <i class="fa-solid fa-chart-pie w-5"></i> Dasbor
                    </button>
                    <button onclick="switchTab('transaksi')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-lg font-medium text-gray-600 hover:bg-gray-100 transition">
                        <i class="fa-solid fa-file-invoice-dollar w-5"></i> Transaksi & Pembukuan
                    </button>
                    <button onclick="switchTab('unit')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-lg font-medium text-gray-600 hover:bg-gray-100 transition">
                        <i class="fa-solid fa-car w-5"></i> Kelola Unit & Foto
                    </button>
                    <button onclick="switchTab('pengaturan')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-lg font-medium text-gray-600 hover:bg-gray-100 transition">
                        <i class="fa-solid fa-gear w-5"></i> Pengaturan
                    </button>
                </nav>
            </div>
            <div class="p-4 border-t">
                <button onclick="handleLogout()" class="w-full flex items-center gap-3 px-4 py-2 text-red-600 font-medium rounded-lg hover:bg-red-50 transition">
                    <i class="fa-solid fa-right-from-bracket w-5"></i> Keluar
                </button>
            </div>
        </aside>

        <!-- KONTEN UTAMA -->
        <main class="flex-1 overflow-y-auto p-8">
            
            <!-- TAB: DASBOR -->
            <section id="tab-dashboard" class="space-y-6">
                <div class="flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
                    <div>
                        <h2 class="text-2xl font-bold">Dasbor Utama</h2>
                        <p class="text-sm text-gray-500">Ringkasan operasional armada dan keuangan rental.</p>
                    </div>
                    <span id="currentDate" class="text-sm bg-white px-4 py-2 rounded-lg border shadow-sm font-medium"></span>
                </div>

                <!-- Kartu Statistik -->
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
                    <div class="bg-white p-6 rounded-xl border shadow-sm flex items-center justify-between">
                        <div>
                            <p class="text-sm text-gray-500 font-medium">Mobil Tersedia</p>
                            <h3 id="statMobilTersedia" class="text-3xl font-black mt-1 text-green-600">0</h3>
                        </div>
                        <div class="w-12 h-12 bg-green-50 text-green-600 rounded-full flex items-center justify-center text-xl"><i class="fa-solid fa-car"></i></div>
                    </div>
                    <div class="bg-white p-6 rounded-xl border shadow-sm flex items-center justify-between">
                        <div>
                            <p class="text-sm text-gray-500 font-medium">Sisa Mobil (Disewa)</p>
                            <h3 id="statSisaMobil" class="text-3xl font-black mt-1 text-amber-500">0</h3>
                        </div>
                        <div class="w-12 h-12 bg-amber-50 text-amber-500 rounded-full flex items-center justify-center text-xl"><i class="fa-solid fa-key"></i></div>
                    </div>
                    <div class="bg-white p-6 rounded-xl border shadow-sm flex items-center justify-between">
                        <div>
                            <p class="text-sm text-gray-500 font-medium">Pemasukan Bulan Ini</p>
                            <h3 id="statPemasukan" class="text-xl font-black mt-1 text-theme">Rp 0</h3>
                        </div>
                        <div class="w-12 h-12 bg-blue-50 text-theme rounded-full flex items-center justify-center text-xl"><i class="fa-solid fa-wallet"></i></div>
                    </div>
                    <div class="bg-white p-6 rounded-xl border shadow-sm flex items-center justify-between">
                        <div>
                            <p class="text-sm text-gray-500 font-medium">Total Denda / Kurang</p>
                            <h3 id="statDendaKekurangan" class="text-xl font-black mt-1 text-red-600">Rp 0</h3>
                        </div>
                        <div class="w-12 h-12 bg-red-50 text-red-600 rounded-full flex items-center justify-center text-xl"><i class="fa-solid fa-triangle-exclamation"></i></div>
                    </div>
                </div>

                <!-- Jadwal & Status Rental Aktif -->
                <div class="bg-white rounded-xl border shadow-sm p-6 space-y-4">
                    <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
                        <div>
                            <h3 class="text-lg font-bold">Jadwal & Status Penyewaan Aktif</h3>
                            <p class="text-xs text-gray-500">Kelola status denda atau kekurangan pembayaran sewa.</p>
                        </div>
                        <button onclick="openModalTransaksi()" class="px-4 py-2 bg-theme text-white text-sm font-bold rounded-lg shadow hover:opacity-90 transition flex items-center gap-2">
                            <i class="fa-solid fa-plus"></i> Tambah Transaksi / Jadwal
                        </button>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-left border-collapse">
                            <thead>
                                <tr class="border-b text-xs uppercase text-gray-400 bg-gray-50">
                                    <th class="p-3">Penyewa</th>
                                    <th class="p-3">Unit Mobil</th>
                                    <th class="p-3">Jadwal Sewa</th>
                                    <th class="p-3">Status Pembayaran</th>
                                    <th class="p-3">Denda / Kekurangan</th>
                                    <th class="p-3 text-center">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="tableJadwalBody" class="text-sm divide-y">
                                <!-- Data Dinamis -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </section>

            <!-- TAB: TRANSAKSI & PEMBUKUAN -->
            <section id="tab-transaksi" class="space-y-6 hidden">
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
                    <div>
                        <h2 class="text-2xl font-bold">Pembukuan & Laporan Bulanan</h2>
                        <p class="text-sm text-gray-500">Rekapitulasi seluruh pemasukan, denda, dan total pembukuan.</p>
                    </div>
                    <button onclick="downloadLaporan()" class="px-4 py-2 bg-emerald-600 text-white text-sm font-bold rounded-lg shadow hover:bg-emerald-700 transition flex items-center gap-2">
                        <i class="fa-solid fa-download"></i> Unduh Laporan (CSV)
                    </button>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-6">
                    <div class="bg-white p-5 rounded-xl border shadow-sm">
                        <span class="text-xs font-bold text-gray-400 uppercase">Total Keseluruhan Pemasukan</span>
                        <h4 id="rekapTotalMasuk" class="text-2xl font-black text-emerald-600 mt-1">Rp 0</h4>
                    </div>
                    <div class="bg-white p-5 rounded-xl border shadow-sm">
                        <span class="text-xs font-bold text-gray-400 uppercase">Total Akumulasi Denda</span>
                        <h4 id="rekapTotalDenda" class="text-2xl font-black text-red-600 mt-1">Rp 0</h4>
                    </div>
                    <div class="bg-white p-5 rounded-xl border shadow-sm">
                        <span class="text-xs font-bold text-gray-400 uppercase">Total Pembukuan Bersih</span>
                        <h4 id="rekapTotalBersih" class="text-2xl font-black text-theme mt-1">Rp 0</h4>
                    </div>
                </div>

                <div class="bg-white rounded-xl border shadow-sm p-6">
                    <h3 class="text-lg font-bold mb-4">Riwayat Lengkap Pembukuan</h3>
                    <div class="overflow-x-auto">
                        <table class="w-full text-left border-collapse">
                            <thead>
                                <tr class="border-b text-xs uppercase text-gray-400 bg-gray-50">
                                    <th class="p-3">Tanggal Mulai</th>
                                    <th class="p-3">Penyewa</th>
                                    <th class="p-3">Unit Disewa</th>
                                    <th class="p-3">Biaya Sewa</th>
                                    <th class="p-3">Denda / Kekurangan</th>
                                    <th class="p-3">Status</th>
                                </tr>
                            </thead>
                            <tbody id="tablePembukuanBody" class="text-sm divide-y">
                                <!-- Data Dinamis -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </section>

            <!-- TAB: KELOLA UNIT -->
            <section id="tab-unit" class="space-y-6 hidden">
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
                    <div>
                        <h2 class="text-2xl font-bold">Kelola Jenis Unit & Foto</h2>
                        <p class="text-sm text-gray-500">Tambah, ubah, atau hapus armada mobil rental Anda.</p>
                    </div>
                    <button onclick="openModalUnit()" class="px-4 py-2 bg-theme text-white text-sm font-bold rounded-lg shadow hover:opacity-90 transition flex items-center gap-2">
                        <i class="fa-solid fa-plus"></i> Tambah Unit Baru
                    </button>
                </div>

                <div id="gridUnit" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                    <!-- Unit Card Dinamis -->
                </div>
            </section>

            <!-- TAB: PENGATURAN -->
            <section id="tab-pengaturan" class="space-y-6 hidden max-w-2xl">
                <div>
                    <h2 class="text-2xl font-bold">Panel Pengaturan Sistem</h2>
                    <p class="text-sm text-gray-500">Kustomisasi identitas rental, akun login, dan tema warna RGB.</p>
                </div>

                <div class="bg-white rounded-xl border shadow-sm p-6">
                    <form onsubmit="saveSettings(event)" class="space-y-6">
                        <div>
                            <h3 class="text-md font-bold mb-3 text-gray-700">Informasi & Akun</h3>
                            <div class="space-y-4">
                                <div>
                                    <label class="block text-sm font-medium mb-1">Nama Rental</label>
                                    <input type="text" id="settingNamaRental" required class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 border-theme ring-theme">
                                </div>
                                <div>
                                    <label class="block text-sm font-medium mb-1">Username Login</label>
                                    <input type="text" id="settingUsername" required class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 border-theme ring-theme">
                                </div>
                                <div>
                                    <label class="block text-sm font-medium mb-1">Kata Sandi Baru</label>
                                    <input type="password" id="settingPassword" placeholder="Kosongkan jika tidak ingin mengubah sandi" class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 border-theme ring-theme">
                                </div>
                            </div>
                        </div>

                        <hr>

                        <div>
                            <h3 class="text-md font-bold mb-1 text-gray-700">Kombinasi Warna Tema (RGB)</h3>
                            <p class="text-xs text-gray-400 mb-3">Atur warna tema sesuai keinginan dengan kombinasi nilai angka 0 s.d 255.</p>
                            <div class="grid grid-cols-3 gap-4">
                                <div>
                                    <label class="block text-xs font-semibold mb-1">Red (Merah)</label>
                                    <input type="number" id="settingColorR" min="0" max="255" required class="w-full px-3 py-2 border rounded-lg" oninput="previewTheme()">
                                </div>
                                <div>
                                    <label class="block text-xs font-semibold mb-1">Green (Hijau)</label>
                                    <input type="number" id="settingColorG" min="0" max="255" required class="w-full px-3 py-2 border rounded-lg" oninput="previewTheme()">
                                </div>
                                <div>
                                    <label class="block text-xs font-semibold mb-1">Blue (Biru)</label>
                                    <input type="number" id="settingColorB" min="0" max="255" required class="w-full px-3 py-2 border rounded-lg" oninput="previewTheme()">
                                </div>
                            </div>
                            <div class="flex items-center gap-3 mt-3">
                                <span class="text-xs text-gray-500 font-medium">Pratinjau Warna Tema:</span>
                                <div id="colorPreviewBox" class="w-16 h-8 rounded-lg border shadow-inner"></div>
                            </div>
                        </div>

                        <button type="submit" class="w-full py-3 bg-theme text-white font-bold rounded-lg shadow-md transition hover:opacity-90">Simpan Perubahan Pengaturan</button>
                    </form>
                </div>
            </section>

        </main>
    </div>

    <!-- MODAL TAMBAH/EDIT UNIT -->
    <div id="modalUnit" class="fixed inset-0 bg-black/50 z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl w-full max-w-md p-6 shadow-xl">
            <h3 id="modalUnitTitle" class="text-xl font-bold mb-4">Tambah Unit Mobil</h3>
            <form onsubmit="saveUnit(event)" class="space-y-4">
                <input type="hidden" id="unitId">
                <div>
                    <label class="block text-sm font-medium mb-1">Nama / Merk Unit Mobil</label>
                    <input type="text" id="unitName" required placeholder="Contoh: Toyota Hiace Premio" class="w-full px-4 py-2 border rounded-lg">
                </div>
                <div>
                    <label class="block text-sm font-medium mb-1">Nomor Polisi (Plat Nomor)</label>
                    <input type="text" id="unitPlat" required placeholder="Contoh: B 1234 XYZ" class="w-full px-4 py-2 border rounded-lg">
                </div>
                <div>
                    <label class="block text-sm font-medium mb-1">Foto Mobil (Upload File Gambar)</label>
                    <input type="file" id="unitFotoFile" accept="image/*" class="w-full px-3 py-2 border rounded-lg text-sm mb-1" onchange="convertImage(this)">
                    <input type="hidden" id="unitFotoBase64">
                </div>
                <div>
                    <label class="block text-sm font-medium mb-1">Status Ketersediaan</label>
                    <select id="unitStatus" class="w-full px-4 py-2 border rounded-lg">
                        <option value="Tersedia">Tersedia</option>
                        <option value="Disewa">Disewa</option>
                    </select>
                </div>
                <div class="flex justify-end gap-2 mt-6">
                    <button type="button" onclick="closeModalUnit()" class="px-4 py-2 border rounded-lg text-gray-600 font-medium">Batal</button>
                    <button type="submit" class="px-4 py-2 bg-theme text-white font-bold rounded-lg">Simpan Unit</button>
                </div>
            </form>
        </div>
    </div>

    <!-- MODAL TAMBAH TRANSAKSI -->
    <div id="modalTransaksi" class="fixed inset-0 bg-black/50 z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl w-full max-w-md p-6 shadow-xl">
            <h3 class="text-xl font-bold mb-4">Tambah Jadwal & Transaksi Rental</h3>
            <form onsubmit="saveTransaksi(event)" class="space-y-4">
                <div>
                    <label class="block text-sm font-medium mb-1">Nama Penyewa</label>
                    <input type="text" id="trxPenyewa" required placeholder="Nama lengkap penyewa" class="w-full px-4 py-2 border rounded-lg">
                </div>
                <div>
                    <label class="block text-sm font-medium mb-1">Pilih Unit Mobil Tersedia</label>
                    <select id="trxUnit" required class="w-full px-4 py-2 border rounded-lg">
                        <!-- Options unit -->
                    </select>
                </div>
                <div class="grid grid-cols-2 gap-2">
                    <div>
                        <label class="block text-sm font-medium mb-1">Mulai Sewa</label>
                        <input type="date" id="trxMulai" required class="w-full px-3 py-2 border rounded-lg text-sm">
                    </div>
                    <div>
                        <label class="block text-sm font-medium mb-1">Selesai Sewa</label>
                        <input type="date" id="trxSelesai" required class="w-full px-3 py-2 border rounded-lg text-sm">
                    </div>
                </div>
                <div>
                    <label class="block text-sm font-medium mb-1">Biaya Sewa (Pemasukan)</label>
                    <input type="number" id="trxBiaya" required placeholder="Contoh: 500000" class="w-full px-4 py-2 border rounded-lg">
                </div>
                <div>
                    <label class="block text-sm font-medium mb-1">Denda / Kekurangan Pembayaran</label>
                    <input type="number" id="trxDenda" value="0" placeholder="0 jika tidak ada" class="w-full px-4 py-2 border rounded-lg">
                </div>
                <div>
                    <label class="block text-sm font-medium mb-1">Status Pembayaran</label>
                    <select id="trxStatusBayar" class="w-full px-4 py-2 border rounded-lg">
                        <option value="Lunas">Lunas</option>
                        <option value="Kurang Pembayaran">Kurang Pembayaran</option>
                        <option value="Ada Denda">Ada Denda</option>
                    </select>
                </div>
                <div class="flex justify-end gap-2 mt-6">
                    <button type="button" onclick="closeModalTransaksi()" class="px-4 py-2 border rounded-lg text-gray-600 font-medium">Batal</button>
                    <button type="submit" class="px-4 py-2 bg-theme text-white font-bold rounded-lg">Simpan Transaksi</button>
                </div>
            </form>
        </div>
    </div>

    <!-- SCRIPT APLIKASI & LOCALSTORAGE -->
    <script>
        const defaultData = {
            config: {
                namaRental: "TEGUH-trans",
                username: "admin",
                password: "123",
                r: 37, g: 99, b: 235
            },
            units: [
                { id: 1, name: "Toyota Hiace Premio", plat: "H 1234 TEG", status: "Tersedia", foto: "https://images.unsplash.com/photo-1549399542-7e3f8b79c341?auto=format&fit=crop&w=400&q=80" },
                { id: 2, name: "Isuzu Elf Long", plat: "B 5678 TRS", status: "Disewa", foto: "https://images.unsplash.com/photo-1563720223185-11003d516935?auto=format&fit=crop&w=400&q=80" }
            ],
            transactions: [
                { id: 1, penyewa: "Ahmad Fauzi", unit: "Isuzu Elf Long", mulai: "2026-08-01", selesai: "2026-08-05", biaya: 3200000, denda: 0, statusBayar: "Lunas" }
            ]
        };

        let db = JSON.parse(localStorage.getItem('teguh_trans_db')) || defaultData;
        let loggedInUser = sessionStorage.getItem('teguh_trans_logged');

        function saveDB() {
            localStorage.setItem('teguh_trans_db', JSON.stringify(db));
        }

        function applyTheme() {
            let cfg = db.config;
            document.documentElement.style.setProperty('--theme-r', cfg.r);
            document.documentElement.style.setProperty('--theme-g', cfg.g);
            document.documentElement.style.setProperty('--theme-b', cfg.b);
            document.getElementById('loginBrandTitle').innerText = cfg.namaRental;
            document.getElementById('sidebarBrandName').innerText = cfg.namaRental;
        }

        window.onload = function() {
            applyTheme();
            if (loggedInUser) {
                document.getElementById('loginPage').classList.add('hidden');
                document.getElementById('appLayout').classList.remove('hidden');
                initDashboard();
            }
        };

        // Login & Sesi
        function handleLogin(e) {
            e.preventDefault();
            let u = document.getElementById('loginUser').value;
            let p = document.getElementById('loginPass').value;
            if (u === db.config.username && p === db.config.password) {
                sessionStorage.setItem('teguh_trans_logged', u);
                document.getElementById('loginPage').classList.add('hidden');
                document.getElementById('appLayout').classList.remove('hidden');
                initDashboard();
            } else {
                alert('Username atau Kata Sandi salah!');
            }
        }

        function handleLogout() {
            sessionStorage.removeItem('teguh_trans_logged');
            location.reload();
        }

        // Navigasi
        function switchTab(tabId) {
            document.querySelectorAll('main > section').forEach(sec => sec.classList.add('hidden'));
            document.getElementById(`tab-${tabId}`).classList.remove('hidden');
            
            document.querySelectorAll('.nav-btn').forEach(btn => btn.classList.remove('bg-gray-100', 'text-theme', 'font-bold'));
            event.currentTarget.classList.add('bg-gray-100', 'text-theme', 'font-bold');

            if(tabId === 'dashboard') initDashboard();
            if(tabId === 'unit') renderUnits();
            if(tabId === 'transaksi') renderPembukuan();
            if(tabId === 'pengaturan') initSettings();
        }

        // Dasbor
        function initDashboard() {
            document.getElementById('currentDate').innerText = new Date().toLocaleDateString('id-ID', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });
            
            let tersedia = db.units.filter(u => u.status === 'Tersedia').length;
            let disewa = db.units.filter(u => u.status === 'Disewa').length;
            
            let totalPemasukan = db.transactions.reduce((acc, curr) => acc + Number(curr.biaya), 0);
            let totalDendaKekurangan = db.transactions.reduce((acc, curr) => acc + Number(curr.denda), 0);

            document.getElementById('statMobilTersedia').innerText = tersedia;
            document.getElementById('statSisaMobil').innerText = disewa;
            document.getElementById('statPemasukan').innerText = 'Rp ' + totalPemasukan.toLocaleString('id-ID');
            document.getElementById('statDendaKekurangan').innerText = 'Rp ' + totalDendaKekurangan.toLocaleString('id-ID');

            let tbody = document.getElementById('tableJadwalBody');
            tbody.innerHTML = '';
            if(db.transactions.length === 0) {
                tbody.innerHTML = `<tr><td colspan="6" class="text-center py-6 text-gray-400">Belum ada jadwal transaksi aktif.</td></tr>`;
                return;
            }

            db.transactions.forEach(t => {
                let badgeColor = t.statusBayar === 'Lunas' ? 'bg-green-100 text-green-700' : 'bg-amber-100 text-amber-700';
                tbody.innerHTML += `
                    <tr class="hover:bg-gray-50">
                        <td class="p-3 font-semibold">${t.penyewa}</td>
                        <td class="p-3">${t.unit}</td>
                        <td class="p-3 text-xs text-gray-500">${t.mulai} s/d ${t.selesai}</td>
                        <td class="p-3"><span class="px-2.5 py-1 rounded-full text-xs font-bold ${badgeColor}">${t.statusBayar}</span></td>
                        <td class="p-3 text-red-600 font-semibold">Rp ${Number(t.denda).toLocaleString('id-ID')}</td>
                        <td class="p-3 text-center">
                            <button onclick="selesaikanTransaksi(${t.id})" title="Selesaikan & Kembalikan Unit" class="px-2.5 py-1 bg-green-50 text-green-600 hover:bg-green-100 rounded text-xs font-bold mr-1"><i class="fa-solid fa-check"></i> Selesai</button>
                            <button onclick="hapusTransaksi(${t.id})" class="text-red-400 hover:text-red-600 px-2 py-1"><i class="fa-solid fa-trash"></i></button>
                        </td>
                    </tr>
                `;
            });
        }

        // Unit & Foto
        function renderUnits() {
            let grid = document.getElementById('gridUnit');
            grid.innerHTML = '';
            db.units.forEach(u => {
                grid.innerHTML += `
                    <div class="bg-white rounded-xl border shadow-sm overflow-hidden flex flex-col justify-between">
                        <div>
                            <img src="${u.foto || 'https://via.placeholder.com/400x200?text=Mobil'}" class="w-full h-44 object-cover">
                            <div class="p-4">
                                <div class="flex justify-between items-start mb-1">
                                    <h4 class="font-bold text-lg">${u.name}</h4>
                                    <span class="px-2 py-0.5 text-xs rounded-full font-bold ${u.status === 'Tersedia' ? 'bg-green-100 text-green-700' : 'bg-amber-100 text-amber-700'}">${u.status}</span>
                                </div>
                                <p class="text-sm text-gray-500"><i class="fa-solid fa-id-card mr-1"></i> ${u.plat}</p>
                            </div>
                        </div>
                        <div class="p-4 border-t bg-gray-50 flex justify-end gap-2">
                            <button onclick="editUnit(${u.id})" class="px-3 py-1.5 bg-gray-200 hover:bg-gray-300 text-xs font-bold rounded-lg">Edit</button>
                            <button onclick="deleteUnit(${u.id})" class="px-3 py-1.5 bg-red-100 hover:bg-red-200 text-red-600 text-xs font-bold rounded-lg">Hapus</button>
                        </div>
                    </div>
                `;
            });
        }

        function openModalUnit() {
            document.getElementById('modalUnitTitle').innerText = "Tambah Unit Mobil";
            document.getElementById('unitId').value = '';
            document.getElementById('unitName').value = '';
            document.getElementById('unitPlat').value = '';
            document.getElementById('unitFotoBase64').value = '';
            document.getElementById('unitFotoFile').value = '';
            document.getElementById('unitStatus').value = 'Tersedia';
            document.getElementById('modalUnit').classList.remove('hidden');
        }

        function closeModalUnit() { document.getElementById('modalUnit').classList.add('hidden'); }

        function convertImage(input) {
            if (input.files && input.files[0]) {
                let reader = new FileReader();
                reader.onload = function(e) {
                    document.getElementById('unitFotoBase64').value = e.target.result;
                }
                reader.readAsDataURL(input.files[0]);
            }
        }

        function saveUnit(e) {
            e.preventDefault();
            let id = document.getElementById('unitId').value;
            let name = document.getElementById('unitName').value;
            let plat = document.getElementById('unitPlat').value;
            let status = document.getElementById('unitStatus').value;
            let foto = document.getElementById('unitFotoBase64').value;

            if (id) {
                let unit = db.units.find(u => u.id == id);
                unit.name = name;
                unit.plat = plat;
                unit.status = status;
                if(foto) unit.foto = foto;
            } else {
                let newId = db.units.length > 0 ? db.units[db.units.length - 1].id + 1 : 1;
                db.units.push({ id: newId, name, plat, status, foto: foto || 'https://images.unsplash.com/photo-1549399542-7e3f8b79c341?auto=format&fit=crop&w=400&q=80' });
            }
            saveDB();
            closeModalUnit();
            renderUnits();
        }

        function editUnit(id) {
            let u = db.units.find(item => item.id == id);
            document.getElementById('modalUnitTitle').innerText = "Edit Unit Mobil";
            document.getElementById('unitId').value = u.id;
            document.getElementById('unitName').value = u.name;
            document.getElementById('unitPlat').value = u.plat;
            document.getElementById('unitStatus').value = u.status;
            document.getElementById('unitFotoBase64').value = u.foto;
            document.getElementById('modalUnit').classList.remove('hidden');
        }

        function deleteUnit(id) {
            if(confirm('Yakin ingin menghapus unit ini dari sistem?')) {
                db.units = db.units.filter(u => u.id != id);
                saveDB();
                renderUnits();
            }
        }

        // Transaksi
        function openModalTransaksi() {
            let select = document.getElementById('trxUnit');
            select.innerHTML = '';
            let availableUnits = db.units.filter(u => u.status === 'Tersedia');
            if(availableUnits.length === 0) {
                alert('Tidak ada unit mobil yang tersedia saat ini.');
                return;
            }
            availableUnits.forEach(u => {
                select.innerHTML += `<option value="${u.name}">${u.name} (${u.plat})</option>`;
            });
            document.getElementById('modalTransaksi').classList.remove('hidden');
        }

        function closeModalTransaksi() { document.getElementById('modalTransaksi').classList.add('hidden'); }

        function saveTransaksi(e) {
            e.preventDefault();
            let penyewa = document.getElementById('trxPenyewa').value;
            let unit = document.getElementById('trxUnit').value;
            let mulai = document.getElementById('trxMulai').value;
            let selesai = document.getElementById('trxSelesai').value;
            let biaya = document.getElementById('trxBiaya').value;
            let denda = document.getElementById('trxDenda').value;
            let statusBayar = document.getElementById('trxStatusBayar').value;

            let newId = db.transactions.length > 0 ? db.transactions[db.transactions.length - 1].id + 1 : 1;
            db.transactions.push({ id: newId, penyewa, unit, mulai, selesai, biaya, denda, statusBayar });

            let targetUnit = db.units.find(u => u.name === unit);
            if(targetUnit) targetUnit.status = "Disewa";

            saveDB();
            closeModalTransaksi();
            initDashboard();
        }

        function selesaikanTransaksi(id) {
            if(confirm('Selesaikan masa rental dan kembalikan unit mobil menjadi Tersedia?')) {
                let t = db.transactions.find(item => item.id == id);
                if(t) {
                    let targetUnit = db.units.find(u => u.name === t.unit);
                    if(targetUnit) targetUnit.status = "Tersedia";
                }
                db.transactions = db.transactions.filter(item => item.id != id);
                saveDB();
                initDashboard();
            }
        }

        function hapusTransaksi(id) {
            if(confirm('Hapus data transaksi ini?')) {
                db.transactions = db.transactions.filter(t => t.id != id);
                saveDB();
                initDashboard();
            }
        }

        // Pembukuan & Unduh Laporan
        function renderPembukuan() {
            let tbody = document.getElementById('tablePembukuanBody');
            tbody.innerHTML = '';
            
            let totalMasuk = db.transactions.reduce((acc, curr) => acc + Number(curr.biaya), 0);
            let totalDenda = db.transactions.reduce((acc, curr) => acc + Number(curr.denda), 0);
            let totalBersih = totalMasuk + totalDenda;

            document.getElementById('rekapTotalMasuk').innerText = 'Rp ' + totalMasuk.toLocaleString('id-ID');
            document.getElementById('rekapTotalDenda').innerText = 'Rp ' + totalDenda.toLocaleString('id-ID');
            document.getElementById('rekapTotalBersih').innerText = 'Rp ' + totalBersih.toLocaleString('id-ID');

            if(db.transactions.length === 0) {
                tbody.innerHTML = `<tr><td colspan="6" class="text-center py-6 text-gray-400">Belum ada riwayat pembukuan.</td></tr>`;
                return;
            }

            db.transactions.forEach(t => {
                tbody.innerHTML += `
                    <tr class="hover:bg-gray-50">
                        <td class="p-3 text-xs text-gray-500">${t.mulai}</td>
                        <td class="p-3 font-semibold">${t.penyewa}</td>
                        <td class="p-3">${t.unit}</td>
                        <td class="p-3 text-emerald-600 font-bold">Rp ${Number(t.biaya).toLocaleString('id-ID')}</td>
                        <td class="p-3 text-red-600 font-bold">Rp ${Number(t.denda).toLocaleString('id-ID')}</td>
                        <td class="p-3"><span class="px-2 py-1 bg-gray-100 rounded text-xs font-bold">${t.statusBayar}</span></td>
                    </tr>
                `;
            });
        }

        function downloadLaporan() {
            let csv = "ID,Tanggal Mulai,Penyewa,Unit,Biaya Sewa,Denda / Kekurangan,Status Pembayaran\n";
            db.transactions.forEach(t => {
                csv += `"${t.id}","${t.mulai}","${t.penyewa}","${t.unit}","${t.biaya}","${t.denda}","${t.statusBayar}"\n`;
            });
            let blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
            let link = document.createElement("a");
            link.href = URL.createObjectURL(blob);
            link.download = `Laporan_Pembukuan_${db.config.namaRental}_${new Date().toISOString().slice(0,7)}.csv`;
            link.click();
        }

        // Pengaturan & Tema RGB
        function initSettings() {
            let cfg = db.config;
            document.getElementById('settingNamaRental').value = cfg.namaRental;
            document.getElementById('settingUsername').value = cfg.username;
            document.getElementById('settingPassword').value = '';
            document.getElementById('settingColorR').value = cfg.r;
            document.getElementById('settingColorG').value = cfg.g;
            document.getElementById('settingColorB').value = cfg.b;
            previewTheme();
        }

        function previewTheme() {
            let r = document.getElementById('settingColorR').value || 0;
            let g = document.getElementById('settingColorG').value || 0;
            let b = document.getElementById('settingColorB').value || 0;
            document.getElementById('colorPreviewBox').style.backgroundColor = `rgb(${r}, ${g}, ${b})`;
        }

        function saveSettings(e) {
            e.preventDefault();
            db.config.namaRental = document.getElementById('settingNamaRental').value;
            db.config.username = document.getElementById('settingUsername').value;
            let newPass = document.getElementById('settingPassword').value;
            if(newPass) db.config.password = newPass;

            db.config.r = Number(document.getElementById('settingColorR').value);
            db.config.g = Number(document.getElementById('settingColorG').value);
            db.config.b = Number(document.getElementById('settingColorB').value);

            saveDB();
            applyTheme();
            alert('Pengaturan berhasil diperbarui!');
        }
    </script>
</body>
</html>
