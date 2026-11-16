<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Database Agenda Surat - SDN Krobokan</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts: Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; }
    </style>
</head>
<body class="bg-gray-50 text-gray-800 antialiased">

    <!-- LOGIN SCREEN -->
    <div id="loginScreen" class="min-h-screen flex items-center justify-center bg-gradient-to-br from-blue-700 via-indigo-800 to-slate-900 p-4">
        <div class="bg-white rounded-2xl shadow-2xl w-full max-w-md p-8 border border-white/20">
            <div class="text-center mb-8">
                <div class="inline-flex items-center justify-center w-16 h-16 bg-blue-100 text-blue-600 rounded-2xl mb-4 shadow-inner">
                    <i class="fa-solid fa-envelope-open-text text-3xl"></i>
                </div>
                <h1 class="text-2xl font-bold text-gray-900">SDN Krobokan</h1>
                <p class="text-sm text-gray-500 mt-1">Sistem Database Agenda Surat Online</p>
            </div>

            <form id="loginForm" onsubmit="handleLogin(event)" class="space-y-5">
                <div>
                    <label class="block text-xs font-semibold uppercase tracking-wider text-gray-600 mb-2">Username / Peran</label>
                    <div class="relative">
                        <span class="absolute inset-y-0 left-0 pl-3.5 flex items-center text-gray-400">
                            <i class="fa-solid fa-user"></i>
                        </span>
                        <input type="text" id="username" required placeholder="Contoh: admin / kepsek / guru" 
                            class="w-full pl-10 pr-4 py-3 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-blue-600 focus:bg-white transition">
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold uppercase tracking-wider text-gray-600 mb-2">Password</label>
                    <div class="relative">
                        <span class="absolute inset-y-0 left-0 pl-3.5 flex items-center text-gray-400">
                            <i class="fa-solid fa-lock"></i>
                        </span>
                        <input type="password" id="password" required placeholder="••••••••" 
                            class="w-full pl-10 pr-4 py-3 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-blue-600 focus:bg-white transition">
                    </div>
                </div>

                <div id="loginError" class="hidden text-red-500 text-xs font-medium text-center bg-red-50 py-2 rounded-lg border border-red-200"></div>

                <button type="submit" class="w-full py-3.5 bg-blue-600 hover:bg-blue-700 text-white font-semibold rounded-xl shadow-lg shadow-blue-600/30 transition duration-200 flex items-center justify-center space-x-2">
                    <span>Masuk Aplikasi</span>
                    <i class="fa-solid fa-arrow-right-to-bracket"></i>
                </button>
            </form>

            <div class="mt-8 pt-6 border-t border-gray-100">
                <p class="text-xs text-gray-400 text-center uppercase tracking-wider mb-3">Akun Demo Akses Cepat:</p>
                <div class="grid grid-cols-3 gap-2">
                    <button onclick="fillDemo('admin', 'admin123')" class="p-2 bg-gray-100 hover:bg-blue-50 hover:text-blue-600 rounded-lg text-xs font-medium transition text-center">
                        <span class="block font-bold">Admin TU</span>admin123
                    </button>
                    <button onclick="fillDemo('kepsek', 'kepsek123')" class="p-2 bg-gray-100 hover:bg-blue-50 hover:text-blue-600 rounded-lg text-xs font-medium transition text-center">
                        <span class="block font-bold">Kepala Sek.</span>kepsek123
                    </button>
                    <button onclick="fillDemo('guru', 'guru123')" class="p-2 bg-gray-100 hover:bg-blue-50 hover:text-blue-600 rounded-lg text-xs font-medium transition text-center">
                        <span class="block font-bold">Guru</span>guru123
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- MAIN APP SCREEN -->
    <div id="appScreen" class="hidden min-h-screen flex flex-col">
        <!-- Top Navbar -->
        <header class="bg-white border-b border-gray-200 sticky top-0 z-30 shadow-xs">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
                <div class="flex items-center space-x-3">
                    <div class="w-10 h-10 bg-blue-600 text-white rounded-xl flex items-center justify-center shadow-md">
                        <i class="fa-solid fa-school text-lg"></i>
                    </div>
                    <div>
                        <h2 class="text-sm font-bold text-gray-900 leading-tight">SDN KROBOKAN</h2>
                        <p class="text-xs text-gray-500">Agenda Surat Online</p>
                    </div>
                </div>

                <div class="flex items-center space-x-4">
                    <div class="hidden sm:flex items-center space-x-2 bg-gray-100 px-3 py-1.5 rounded-full border border-gray-200">
                        <i class="fa-solid fa-calendar-days text-blue-600 text-xs"></i>
                        <span id="activeAcademicYear" class="text-xs font-semibold text-gray-700">TA: 2026/2027</span>
                    </div>

                    <div class="flex items-center space-x-3 border-l border-gray-200 pl-4">
                        <div class="text-right hidden md:block">
                            <div id="userFullName" class="text-xs font-bold text-gray-800">Admin TU</div>
                            <div id="userRoleBadge" class="text-[10px] font-semibold px-2 py-0.5 bg-blue-100 text-blue-700 rounded-full inline-block">Admin</div>
                        </div>
                        <button onclick="handleLogout()" class="p-2 bg-red-50 hover:bg-red-100 text-red-600 rounded-xl transition text-sm" title="Keluar">
                            <i class="fa-solid fa-power-off"></i>
                        </button>
                    </div>
                </div>
            </div>
        </header>

        <!-- Main Content Area -->
        <div class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6">
            
            <!-- Navigation Tabs -->
            <div class="flex space-x-2 border-b border-gray-200 mb-6 overflow-x-auto pb-1">
                <button onclick="switchTab('dashboard')" id="tab-dashboard" class="tab-btn px-4 py-2.5 text-sm font-semibold rounded-t-xl transition flex items-center space-x-2 bg-blue-600 text-white shadow-sm">
                    <i class="fa-solid fa-chart-pie"></i>
                    <span>Dashboard</span>
                </button>
                <button onclick="switchTab('surat-masuk')" id="tab-surat-masuk" class="tab-btn px-4 py-2.5 text-sm font-semibold rounded-t-xl transition flex items-center space-x-2 bg-white text-gray-600 hover:bg-gray-100">
                    <i class="fa-solid fa-inbox"></i>
                    <span>Surat Masuk</span>
                </button>
                <button onclick="switchTab('surat-keluar')" id="tab-surat-keluar" class="tab-btn px-4 py-2.5 text-sm font-semibold rounded-t-xl transition flex items-center space-x-2 bg-white text-gray-600 hover:bg-gray-100">
                    <i class="fa-solid fa-paper-plane"></i>
                    <span>Surat Keluar</span>
                </button>
                <button onclick="switchTab('panduan')" id="tab-panduan" class="tab-btn px-4 py-2.5 text-sm font-semibold rounded-t-xl transition flex items-center space-x-2 bg-white text-gray-600 hover:bg-gray-100">
                    <i class="fa-solid fa-globe"></i>
                    <span>Akses Online & HP</span>
                </button>
            </div>

            <!-- TAB 1: DASHBOARD -->
            <div id="view-dashboard" class="space-y-6">
                <!-- Welcome Banner -->
                <div class="bg-gradient-to-r from-blue-600 to-indigo-700 rounded-2xl p-6 text-white shadow-xl relative overflow-hidden">
                    <div class="relative z-10">
                        <h2 class="text-xl sm:text-2xl font-bold mb-2">Selamat Datang di Sistem Agenda Surat SDN Krobokan</h2>
                        <p class="text-blue-100 text-sm max-w-2xl">
                            Aplikasi arsip dan agenda surat digital yang dapat diakses dengan mudah melalui komputer, tablet, maupun handphone oleh Admin TU, Dewan Guru, dan Kepala Sekolah.
                        </p>
                    </div>
                    <div class="absolute right-[-20px] bottom-[-20px] text-white/10 text-9xl pointer-events-none">
                        <i class="fa-solid fa-folder-open"></i>
                    </div>
                </div>

                <!-- Stats Grid -->
                <div class="grid grid-cols-1 sm:grid-cols-3 gap-5">
                    <div class="bg-white p-6 rounded-2xl border border-gray-200 shadow-sm flex items-center justify-between">
                        <div>
                            <p class="text-xs font-bold uppercase tracking-wider text-gray-400">Total Surat Masuk</p>
                            <h3 id="statSuratMasuk" class="text-3xl font-extrabold text-gray-900 mt-1">0</h3>
                        </div>
                        <div class="w-12 h-12 bg-blue-50 text-blue-600 rounded-xl flex items-center justify-center text-xl">
                            <i class="fa-solid fa-inbox"></i>
                        </div>
                    </div>
                    <div class="bg-white p-6 rounded-2xl border border-gray-200 shadow-sm flex items-center justify-between">
                        <div>
                            <p class="text-xs font-bold uppercase tracking-wider text-gray-400">Total Surat Keluar</p>
                            <h3 id="statSuratKeluar" class="text-3xl font-extrabold text-gray-900 mt-1">0</h3>
                        </div>
                        <div class="w-12 h-12 bg-emerald-50 text-emerald-600 rounded-xl flex items-center justify-center text-xl">
                            <i class="fa-solid fa-paper-plane"></i>
                        </div>
                    </div>
                    <div class="bg-white p-6 rounded-2xl border border-gray-200 shadow-sm flex items-center justify-between">
                        <div>
                            <p class="text-xs font-bold uppercase tracking-wider text-gray-400">Tahun Ajaran Aktif</p>
                            <h3 class="text-2xl font-extrabold text-gray-900 mt-1">2026/2027</h3>
                        </div>
                        <div class="w-12 h-12 bg-amber-50 text-amber-600 rounded-xl flex items-center justify-center text-xl">
                            <i class="fa-solid fa-calendar-check"></i>
                        </div>
                    </div>
                </div>

                <!-- Quick Overview Table / Activity -->
                <div class="bg-white rounded-2xl border border-gray-200 shadow-sm p-6">
                    <div class="flex items-center justify-between mb-4">
                        <h3 class="text-base font-bold text-gray-900">Agenda Surat Terbaru</h3>
                        <button onclick="switchTab('surat-masuk')" class="text-xs text-blue-600 font-semibold hover:underline">Lihat Semua →</button>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-sm">
                            <thead class="bg-gray-50 text-gray-500 uppercase text-xs">
                                <tr>
                                    <th class="p-3">No. Agenda</th>
                                    <th class="p-3">Jenis</th>
                                    <th class="p-3">No. Surat</th>
                                    <th class="p-3">Perihal / Tujuan</th>
                                    <th class="p-3">Tanggal</th>
                                </tr>
                            </thead>
                            <tbody id="recentActivityTable" class="divide-y divide-gray-100">
                                <!-- Populated by JS -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- TAB 2: SURAT MASUK -->
            <div id="view-surat-masuk" class="hidden space-y-6">
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 bg-white p-4 rounded-2xl border border-gray-200 shadow-sm">
                    <div>
                        <h3 class="text-lg font-bold text-gray-900">Data Surat Masuk</h3>
                        <p class="text-xs text-gray-500">Kelola dan pantau seluruh surat masuk SDN Krobokan</p>
                    </div>
                    <div class="flex items-center space-x-3 w-full sm:w-auto">
                        <input type="text" id="searchSuratMasuk" oninput="renderSuratMasuk()" placeholder="Cari surat..." 
                            class="px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-blue-600 w-full sm:w-64">
                        <button id="btnTambahSuratMasuk" onclick="openModalSuratMasuk()" class="hidden px-4 py-2 bg-blue-600 hover:bg-blue-700 text-white text-sm font-semibold rounded-xl shadow-md transition flex items-center space-x-2 shrink-0">
                            <i class="fa-solid fa-plus"></i>
                            <span>Tambah</span>
                        </button>
                    </div>
                </div>

                <div class="bg-white rounded-2xl border border-gray-200 shadow-sm overflow-hidden">
                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-sm">
                            <thead class="bg-gray-50 text-gray-500 uppercase text-xs">
                                <tr>
                                    <th class="p-4">Agenda</th>
                                    <th class="p-4">No. Surat & Tgl</th>
                                    <th class="p-4">Pengirim</th>
                                    <th class="p-4">Perihal</th>
                                    <th class="p-4">File / Lampiran</th>
                                    <th class="p-4 text-center admin-only">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="tableSuratMasukBody" class="divide-y divide-gray-100">
                                <!-- Populated by JS -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- TAB 3: SURAT KELUAR -->
            <div id="view-surat-keluar" class="hidden space-y-6">
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 bg-white p-4 rounded-2xl border border-gray-200 shadow-sm">
                    <div>
                        <h3 class="text-lg font-bold text-gray-900">Data Surat Keluar</h3>
                        <p class="text-xs text-gray-500">Kelola arsip surat keluar SDN Krobokan</p>
                    </div>
                    <div class="flex items-center space-x-3 w-full sm:w-auto">
                        <input type="text" id="searchSuratKeluar" oninput="renderSuratKeluar()" placeholder="Cari surat..." 
                            class="px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-blue-600 w-full sm:w-64">
                        <button id="btnTambahSuratKeluar" onclick="openModalSuratKeluar()" class="hidden px-4 py-2 bg-emerald-600 hover:bg-emerald-700 text-white text-sm font-semibold rounded-xl shadow-md transition flex items-center space-x-2 shrink-0">
                            <i class="fa-solid fa-plus"></i>
                            <span>Tambah</span>
                        </button>
                    </div>
                </div>

                <div class="bg-white rounded-2xl border border-gray-200 shadow-sm overflow-hidden">
                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-sm">
                            <thead class="bg-gray-50 text-gray-500 uppercase text-xs">
                                <tr>
                                    <th class="p-4">Agenda</th>
                                    <th class="p-4">No. Surat & Tgl</th>
                                    <th class="p-4">Tujuan</th>
                                    <th class="p-4">Perihal</th>
                                    <th class="p-4">File / Lampiran</th>
                                    <th class="p-4 text-center admin-only">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="tableSuratKeluarBody" class="divide-y divide-gray-100">
                                <!-- Populated by JS -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- TAB 4: PANDUAN AKSES ONLINE & HP -->
            <div id="view-panduan" class="hidden space-y-6">
                <div class="bg-white rounded-2xl border border-gray-200 shadow-sm p-6 sm:p-8 space-y-6">
                    <div class="border-b border-gray-100 pb-4">
                        <h3 class="text-xl font-bold text-gray-900 flex items-center space-x-3">
                            <i class="fa-solid fa-mobile-screen-button text-blue-600"></i>
                            <span>Panduan Akses Online (Komputer & HP)</span>
                        </h3>
                        <p class="text-sm text-gray-500 mt-1">Agar database surat SDN Krobokan dapat diakses kapanpun dan dimanapun oleh Admin, Guru, dan Kepala Sekolah secara real-time.</p>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                        <div class="bg-blue-50/50 p-5 rounded-2xl border border-blue-100 space-y-3">
                            <div class="w-10 h-10 bg-blue-600 text-white rounded-xl flex items-center justify-center font-bold">1</div>
                            <h4 class="font-bold text-gray-900 text-sm">Upload ke GitHub Pages / Netlify</h4>
                            <p class="text-xs text-gray-600 leading-relaxed">
                                File web ini dapat diunggah secara gratis ke layanan hosting seperti <b>Netlify, Vercel, atau GitHub Pages</b>. Hanya butuh waktu 1 menit (cukup drag & drop file HTML ini).
                            </p>
                        </div>

                        <div class="bg-emerald-50/50 p-5 rounded-2xl border border-emerald-100 space-y-3">
                            <div class="w-10 h-10 bg-emerald-600 text-white rounded-xl flex items-center justify-center font-bold">2</div>
                            <h4 class="font-bold text-gray-900 text-sm">Akses via Browser HP / Komputer</h4>
                            <p class="text-xs text-gray-600 leading-relaxed">
                                Setelah online, Admin, Guru, dan Kepala Sekolah cukup membuka link website di browser HP (Chrome/Safari) atau Laptop tanpa perlu install aplikasi tambahan.
                            </p>
                        </div>

                        <div class="bg-purple-50/50 p-5 rounded-2xl border border-purple-100 space-y-3">
                            <div class="w-10 h-10 bg-purple-600 text-white rounded-xl flex items-center justify-center font-bold">3</div>
                            <h4 class="font-bold text-gray-900 text-sm">Hak Akses Peran (Role-Based)</h4>
                            <p class="text-xs text-gray-600 leading-relaxed">
                                <b>Admin TU:</b> Dapat menambah, mengedit, dan menghapus surat.<br>
                                <b>Guru & KS:</b> Dapat melihat, memfilter, dan membaca surat & lampiran dokumen.
                            </p>
                        </div>
                    </div>

                    <div class="bg-gray-50 p-5 rounded-2xl border border-gray-200">
                        <h4 class="font-bold text-gray-900 text-sm mb-2 flex items-center space-x-2">
                            <i class="fa-solid fa-circle-info text-blue-600"></i>
                            <span>Cara Simpan ke Home Screen HP (Shortcut)</span>
                        </h4>
                        <ol class="list-decimal list-inside text-xs text-gray-600 space-y-1.5">
                            <li>Buka link web aplikasi di browser HP (Google Chrome / Safari).</li>
                            <li>Ketuk ikon titik tiga di sudut kanan atas browser (Chrome) atau ikon Share (Safari).</li>
                            <li>Pilih menu <b>"Tambahkan ke Layar Utama" (Add to Home Screen)</b>.</li>
                            <li>Aplikasi akan muncul di menu HP layaknya aplikasi android/iOS biasa.</li>
                        </ol>
                    </div>
                </div>
            </div>

        </div>
    </div>

    <!-- MODAL TAMBAH / EDIT SURAT MASUK -->
    <div id="modalSuratMasuk" class="hidden fixed inset-0 bg-black/50 z-50 flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl shadow-2xl w-full max-w-lg p-6 space-y-5 max-h-[90vh] overflow-y-auto">
            <div class="flex justify-between items-center border-b border-gray-100 pb-3">
                <h3 id="modalSuratMasukTitle" class="text-lg font-bold text-gray-900">Tambah Surat Masuk</h3>
                <button onclick="closeModalSuratMasuk()" class="text-gray-400 hover:text-gray-600"><i class="fa-solid fa-xmark text-lg"></i></button>
            </div>
            <form id="formSuratMasuk" onsubmit="saveSuratMasuk(event)" class="space-y-4">
                <input type="hidden" id="sm_id">
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-semibold text-gray-600 mb-1">No. Agenda</label>
                        <input type="text" id="sm_no_agenda" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:ring-2 focus:ring-blue-600">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-600 mb-1">Tahun Ajaran</label>
                        <select id="sm_tahun_ajaran" class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:ring-2 focus:ring-blue-600">
                            <option value="2026/2027">2026/2027</option>
                            <option value="2025/2026">2025/2026</option>
                            <option value="2024/2025">2024/2025</option>
                        </select>
                    </div>
                </div>
                <div>
                    <label class="block text-xs font-semibold text-gray-600 mb-1">No. Surat</label>
                    <input type="text" id="sm_no_surat" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:ring-2 focus:ring-blue-600">
                </div>
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-semibold text-gray-600 mb-1">Tanggal Surat</label>
                        <input type="date" id="sm_tgl_surat" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:ring-2 focus:ring-blue-600">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-600 mb-1">Tanggal Terima</label>
                        <input type="date" id="sm_tgl_terima" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:ring-2 focus:ring-blue-600">
                    </div>
                </div>
                <div>
                    <label class="block text-xs font-semibold text-gray-600 mb-1">Pengirim</label>
                    <input type="text" id="sm_pengirim" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:ring-2 focus:ring-blue-600">
                </div>
                <div>
                    <label class="block text-xs font-semibold text-gray-600 mb-1">Perihal</label>
                    <textarea id="sm_perihal" rows="2" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:ring-2 focus:ring-blue-600"></textarea>
                </div>
                <div>
                    <label class="block text-xs font-semibold text-gray-600 mb-1">Link File / Google Drive (URL)</label>
                    <input type="url" id="sm_file_url" placeholder="https://drive.google.com/..." class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:ring-2 focus:ring-blue-600">
                </div>
                <div class="flex justify-end space-x-3 pt-3">
                    <button type="button" onclick="closeModalSuratMasuk()" class="px-4 py-2 bg-gray-100 text-gray-600 font-semibold text-sm rounded-xl hover:bg-gray-200">Batal</button>
                    <button type="submit" class="px-5 py-2 bg-blue-600 text-white font-semibold text-sm rounded-xl hover:bg-blue-700 shadow-md">Simpan Data</button>
                </div>
            </form>
        </div>
    </div>

    <!-- MODAL TAMBAH / EDIT SURAT KELUAR -->
    <div id="modalSuratKeluar" class="hidden fixed inset-0 bg-black/50 z-50 flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl shadow-2xl w-full max-w-lg p-6 space-y-5 max-h-[90vh] overflow-y-auto">
            <div class="flex justify-between items-center border-b border-gray-100 pb-3">
                <h3 id="modalSuratKeluarTitle" class="text-lg font-bold text-gray-900">Tambah Surat Keluar</h3>
                <button onclick="closeModalSuratKeluar()" class="text-gray-400 hover:text-gray-600"><i class="fa-solid fa-xmark text-lg"></i></button>
            </div>
            <form id="formSuratKeluar" onsubmit="saveSuratKeluar(event)" class="space-y-4">
                <input type="hidden" id="sk_id">
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-semibold text-gray-600 mb-1">No. Agenda</label>
                        <input type="text" id="sk_no_agenda" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:ring-2 focus:ring-emerald-600">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-600 mb-1">Tahun Ajaran</label>
                        <select id="sk_tahun_ajaran" class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:ring-2 focus:ring-emerald-600">
                            <option value="2026/2027">2026/2027</option>
                            <option value="2025/2026">2025/2026</option>
                            <option value="2024/2025">2024/2025</option>
                        </select>
                    </div>
                </div>
                <div>
                    <label class="block text-xs font-semibold text-gray-600 mb-1">No. Surat</label>
                    <input type="text" id="sk_no_surat" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:ring-2 focus:ring-emerald-600">
                </div>
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-semibold text-gray-600 mb-1">Tanggal Surat</label>
                        <input type="date" id="sk_tgl_surat" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:ring-2 focus:ring-emerald-600">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-600 mb-1">Tujuan / Kepada</label>
                        <input type="text" id="sk_tujuan" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:ring-2 focus:ring-emerald-600">
                    </div>
                </div>
                <div>
                    <label class="block text-xs font-semibold text-gray-600 mb-1">Perihal</label>
                    <textarea id="sk_perihal" rows="2" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:ring-2 focus:ring-emerald-600"></textarea>
                </div>
                <div>
                    <label class="block text-xs font-semibold text-gray-600 mb-1">Link File / Google Drive (URL)</label>
                    <input type="url" id="sk_file_url" placeholder="https://drive.google.com/..." class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-xl text-sm focus:ring-2 focus:ring-emerald-600">
                </div>
                <div class="flex justify-end space-x-3 pt-3">
                    <button type="button" onclick="closeModalSuratKeluar()" class="px-4 py-2 bg-gray-100 text-gray-600 font-semibold text-sm rounded-xl hover:bg-gray-200">Batal</button>
                    <button type="submit" class="px-5 py-2 bg-emerald-600 text-white font-semibold text-sm rounded-xl hover:bg-emerald-700 shadow-md">Simpan Data</button>
                </div>
            </form>
        </div>
    </div>

    <!-- JAVASCRIPT APP LOGIC -->
    <script>
        // Initial Database Store
        let currentUser = JSON.parse(localStorage.getItem('sdn_krob_user')) || null;

        let suratMasukData = JSON.parse(localStorage.getItem('sdn_krob_sm')) || [
            {
                id: "SM-1",
                no_agenda: "118",
                no_surat: "800.1.11.1/1189/2026",
                tgl_surat: "2026-11-06",
                tgl_terima: "2026-11-09",
                pengirim: "Dinas Pendidikan Kota Semarang",
                perihal: "Surat Tugas Kegiatan Peningkatan Kompetensi Guru SD dalam mengajar Bahasa Inggris (PKGSD-MBI) Jawa Tengah 2026",
                tahun_ajaran: "2026/2027",
                file_url: "https://drive.google.com/file/d/1LIzkE1vQHInFpTfm436QCWPWnzmHQ0JU/view?usp=sharing"
            },
            {
                id: "SM-2",
                no_agenda: "119",
                no_surat: "B/2551/400.3.10/XI/2026",
                tgl_surat: "2026-11-04",
                tgl_terima: "2026-11-09",
                pengirim: "Inspektorat Kota Semarang",
                perihal: "Kegiatan Sekolah Jujur Sekolah Saya (SJSS) “Guru Kreatif Menciptakan Dampak” Sesi 3",
                tahun_ajaran: "2026/2027",
                file_url: "https://drive.google.com/file/d/1qxSKPjPbHiPjUKIADh1rGUSeXZxb7Vrh/view?usp=drive_link"
            }
        ];

        let suratKeluarData = JSON.parse(localStorage.getItem('sdn_krob_sk')) || [
            {
                id: "SK-1",
                no_agenda: "117",
                no_surat: "117/400.3.5.08/SDN.Krb/XI/2026",
                tgl_surat: "2026-11-05",
                tujuan: "Pratanda Pandeana Putri S., S.Pd",
                perihal: "Surat Penugasan",
                tahun_ajaran: "2026/2027",
                file_url: "https://drive.google.com/file/d/1VR9OirwPdR3sLp9jfz9AuO6zwo1LajR2/view?usp=sharing"
            },
            {
                id: "SK-2",
                no_agenda: "118",
                no_surat: "118/400.3.5.08/SDN.Krb/XI/2026",
                tgl_surat: "2026-11-09",
                tujuan: "Lina Insa Mayasari, S.Pd",
                perihal: "Surat Penugasan",
                tahun_ajaran: "2026/2027",
                file_url: "https://drive.google.com/file/d/1xXUyJDm-NUrFs4X2cVnBgoUXQpTvna2E/view?usp=sharing"
            },
            {
                id: "SK-3",
                no_agenda: "119",
                no_surat: "119/400.3.5.08/SDN.Krb/XI/2026",
                tgl_surat: "2026-11-09",
                tujuan: "Vita Nur Fatimah, S.Pd",
                perihal: "Surat Penugasan",
                tahun_ajaran: "2026/2027",
                file_url: "https://drive.google.com/file/d/1xIYEjoFJOzZHX4KC_OLVVfWJWziYnkNU/view?usp=drive_link"
            }
        ];

        // Init Check
        window.addEventListener('DOMContentLoaded', () => {
            if (currentUser) {
                showApp();
            }
        });

        function fillDemo(u, p) {
            document.getElementById('username').value = u;
            document.getElementById('password').value = p;
        }

        function handleLogin(e) {
            e.preventDefault();
            const u = document.getElementById('username').value.trim().toLowerCase();
            const p = document.getElementById('password').value.trim();
            const errDiv = document.getElementById('loginError');

            let roleName = "";
            let fullName = "";

            if (u === 'admin' && p === 'admin123') {
                roleName = 'Admin';
                fullName = 'Admin TU SDN Krobokan';
            } else if (u === 'kepsek' && p === 'kepsek123') {
                roleName = 'Viewer';
                fullName = 'Kepala Sekolah';
            } else if (u === 'guru' && p === 'guru123') {
                roleName = 'Viewer';
                fullName = 'Dewan Guru SDN Krobokan';
            } else {
                errDiv.textContent = 'Username atau password salah! (Gunakan demo di bawah)';
                errDiv.classList.remove('hidden');
                return;
            }

            currentUser = { username: u, nama_lengkap: fullName, role: roleName };
            localStorage.setItem('sdn_krob_user', JSON.stringify(currentUser));
            showApp();
        }

        function handleLogout() {
            localStorage.removeItem('sdn_krob_user');
            currentUser = null;
            document.getElementById('appScreen').classList.add('hidden');
            document.getElementById('loginScreen').classList.remove('hidden');
            document.getElementById('loginForm').reset();
            document.getElementById('loginError').classList.add('hidden');
        }

        function showApp() {
            document.getElementById('loginScreen').classList.add('hidden');
            document.getElementById('appScreen').classList.remove('hidden');
            document.getElementById('userFullName').textContent = currentUser.nama_lengkap;
            document.getElementById('userRoleBadge').textContent = currentUser.role === 'Admin' ? 'Admin TU' : 'Viewer / Staff';

            if (currentUser.role === 'Admin') {
                document.querySelectorAll('.admin-only').forEach(el => el.classList.remove('hidden'));
                document.getElementById('btnTambahSuratMasuk').classList.remove('hidden');
                document.getElementById('btnTambahSuratKeluar').classList.remove('hidden');
            } else {
                document.querySelectorAll('.admin-only').forEach(el => el.classList.add('hidden'));
                document.getElementById('btnTambahSuratMasuk').classList.add('hidden');
                document.getElementById('btnTambahSuratKeluar').classList.add('hidden');
            }

            updateDashboardStats();
            renderSuratMasuk();
            renderSuratKeluar();
            renderRecentActivity();
        }

        function switchTab(tabId) {
            ['dashboard', 'surat-masuk', 'surat-keluar', 'panduan'].forEach(t => {
                document.getElementById(`view-${t}`).classList.add('hidden');
                const btn = document.getElementById(`tab-${t}`);
                btn.className = "tab-btn px-4 py-2.5 text-sm font-semibold rounded-t-xl transition flex items-center space-x-2 bg-white text-gray-600 hover:bg-gray-100";
            });

            document.getElementById(`view-${tabId}`).classList.remove('hidden');
            const activeBtn = document.getElementById(`tab-${tabId}`);
            activeBtn.className = "tab-btn px-4 py-2.5 text-sm font-semibold rounded-t-xl transition flex items-center space-x-2 bg-blue-600 text-white shadow-sm";
        }

        function updateDashboardStats() {
            document.getElementById('statSuratMasuk').textContent = suratMasukData.length;
            document.getElementById('statSuratKeluar').textContent = suratKeluarData.length;
        }

        function renderRecentActivity() {
            const tbody = document.getElementById('recentActivityTable');
            let html = '';
            
            const combined = [
                ...suratMasukData.map(s => ({...s, jenis: 'Masuk'})),
                ...suratKeluarData.map(s => ({...s, jenis: 'Keluar'}))
            ].sort((a,b) => new Date(b.tgl_surat) - new Date(a.tgl_surat)).slice(0, 5);

            if (combined.length === 0) {
                tbody.innerHTML = `<tr><td colspan="5" class="p-4 text-center text-gray-400">Belum ada agenda surat.</td></tr>`;
                return;
            }

            combined.forEach(item => {
                const badgeColor = item.jenis === 'Masuk' ? 'bg-blue-100 text-blue-700' : 'bg-emerald-100 text-emerald-700';
                html += `
                    <tr class="hover:bg-gray-50">
                        <td class="p-3 font-semibold">#${item.no_agenda}</td>
                        <td class="p-3"><span class="px-2.5 py-1 rounded-full text-xs font-bold ${badgeColor}">${item.jenis}</span></td>
                        <td class="p-3 font-mono text-xs">${item.no_surat}</td>
                        <td class="p-3 text-gray-600 truncate max-w-xs">${item.perihal || item.tujuan}</td>
                        <td class="p-3 text-gray-500 text-xs">${item.tgl_surat}</td>
                    </tr>
                `;
            });
            tbody.innerHTML = html;
        }

        function renderSuratMasuk() {
            const keyword = document.getElementById('searchSuratMasuk').value.toLowerCase();
            const tbody = document.getElementById('tableSuratMasukBody');
            const filtered = suratMasukData.filter(s => 
                s.no_surat.toLowerCase().includes(keyword) || 
                s.pengirim.toLowerCase().includes(keyword) || 
                s.perihal.toLowerCase().includes(keyword) ||
                s.no_agenda.includes(keyword)
            );

            if (filtered.length === 0) {
                tbody.innerHTML = `<tr><td colspan="6" class="p-6 text-center text-gray-400">Tidak ada data surat masuk ditemukan.</td></tr>`;
                return;
            }

            let html = '';
            filtered.forEach(item => {
                let fileBtn = item.file_url ? `<a href="${item.file_url}" target="_blank" class="px-3 py-1 bg-blue-50 text-blue-600 hover:bg-blue-100 rounded-lg text-xs font-semibold inline-flex items-center space-x-1.5"><i class="fa-solid fa-eye"></i><span>Buka File</span></a>` : `<span class="text-xs text-gray-400">Tidak ada file</span>`;
                let actionBtns = currentUser.role === 'Admin' ? `
                    <div class="flex items-center justify-center space-x-2">
                        <button onclick="editSuratMasuk('${item.id}')" class="p-1.5 bg-amber-50 text-amber-600 hover:bg-amber-100 rounded-lg text-xs" title="Edit"><i class="fa-solid fa-pen"></i></button>
                        <button onclick="deleteSuratMasuk('${item.id}')" class="p-1.5 bg-red-50 text-red-600 hover:bg-red-100 rounded-lg text-xs" title="Hapus"><i class="fa-solid fa-trash"></i></button>
                    </div>
                ` : `<span class="text-xs text-gray-400">-</span>`;

                html += `
                    <tr class="hover:bg-gray-50/80">
                        <td class="p-4 font-bold text-gray-900">#${item.no_agenda}</td>
                        <td class="p-4">
                            <div class="font-mono text-xs font-semibold text-blue-600">${item.no_surat}</div>
                            <div class="text-[11px] text-gray-400 mt-0.5">Tgl: ${item.tgl_surat} (Diterima: ${item.tgl_terima})</div>
                        </td>
                        <td class="p-4 text-xs font-medium text-gray-700">${item.pengirim}</td>
                        <td class="p-4 text-xs text-gray-600 max-w-xs leading-relaxed">${item.perihal}</td>
                        <td class="p-4">${fileBtn}</td>
                        <td class="p-4 text-center admin-only">${actionBtns}</td>
                    </tr>
                `;
            });
            tbody.innerHTML = html;
        }

        function renderSuratKeluar() {
            const keyword = document.getElementById('searchSuratKeluar').value.toLowerCase();
            const tbody = document.getElementById('tableSuratKeluarBody');
            const filtered = suratKeluarData.filter(s => 
                s.no_surat.toLowerCase().includes(keyword) || 
                s.tujuan.toLowerCase().includes(keyword) || 
                s.perihal.toLowerCase().includes(keyword) ||
                s.no_agenda.includes(keyword)
            );

            if (filtered.length === 0) {
                tbody.innerHTML = `<tr><td colspan="6" class="p-6 text-center text-gray-400">Tidak ada data surat keluar ditemukan.</td></tr>`;
                return;
            }

            let html = '';
            filtered.forEach(item => {
                let fileBtn = item.file_url ? `<a href="${item.file_url}" target="_blank" class="px-3 py-1 bg-emerald-50 text-emerald-600 hover:bg-emerald-100 rounded-lg text-xs font-semibold inline-flex items-center space-x-1.5"><i class="fa-solid fa-eye"></i><span>Buka File</span></a>` : `<span class="text-xs text-gray-400">Tidak ada file</span>`;
                let actionBtns = currentUser.role === 'Admin' ? `
                    <div class="flex items-center justify-center space-x-2">
                        <button onclick="editSuratKeluar('${item.id}')" class="p-1.5 bg-amber-50 text-amber-600 hover:bg-amber-100 rounded-lg text-xs" title="Edit"><i class="fa-solid fa-pen"></i></button>
                        <button onclick="deleteSuratKeluar('${item.id}')" class="p-1.5 bg-red-50 text-red-600 hover:bg-red-100 rounded-lg text-xs" title="Hapus"><i class="fa-solid fa-trash"></i></button>
                    </div>
                ` : `<span class="text-xs text-gray-400">-</span>`;

                html += `
                    <tr class="hover:bg-gray-50/80">
                        <td class="p-4 font-bold text-gray-900">#${item.no_agenda}</td>
                        <td class="p-4">
                            <div class="font-mono text-xs font-semibold text-emerald-600">${item.no_surat}</div>
                            <div class="text-[11px] text-gray-400 mt-0.5">Tgl: ${item.tgl_surat}</div>
                        </td>
                        <td class="p-4 text-xs font-medium text-gray-700">${item.tujuan}</td>
                        <td class="p-4 text-xs text-gray-600 max-w-xs leading-relaxed">${item.perihal}</td>
                        <td class="p-4">${fileBtn}</td>
                        <td class="p-4 text-center admin-only">${actionBtns}</td>
                    </tr>
                `;
            });
            tbody.innerHTML = html;
        }

        // Modal Surat Masuk
        function openModalSuratMasuk() {
            document.getElementById('modalSuratMasukTitle').textContent = 'Tambah Surat Masuk';
            document.getElementById('formSuratMasuk').reset();
            document.getElementById('sm_id').value = '';
            document.getElementById('modalSuratMasuk').classList.remove('hidden');
        }
        function closeModalSuratMasuk() {
            document.getElementById('modalSuratMasuk').classList.add('hidden');
        }
        function saveSuratMasuk(e) {
            e.preventDefault();
            const id = document.getElementById('sm_id').value;
            const newData = {
                id: id || 'SM-' + Date.now(),
                no_agenda: document.getElementById('sm_no_agenda').value,
                no_surat: document.getElementById('sm_no_surat').value,
                tgl_surat: document.getElementById('sm_tgl_surat').value,
                tgl_terima: document.getElementById('sm_tgl_terima').value,
                pengirim: document.getElementById('sm_pengirim').value,
                perihal: document.getElementById('sm_perihal').value,
                tahun_ajaran: document.getElementById('sm_tahun_ajaran').value,
                file_url: document.getElementById('sm_file_url').value
            };

            if (id) {
                const idx = suratMasukData.findIndex(s => s.id === id);
                if (idx !== -1) suratMasukData[idx] = newData;
            } else {
                suratMasukData.unshift(newData);
            }

            localStorage.setItem('sdn_krob_sm', JSON.stringify(suratMasukData));
            closeModalSuratMasuk();
            renderSuratMasuk();
            updateDashboardStats();
            renderRecentActivity();
        }
        function editSuratMasuk(id) {
            const item = suratMasukData.find(s => s.id === id);
            if (!item) return;
            document.getElementById('modalSuratMasukTitle').textContent = 'Edit Surat Masuk';
            document.getElementById('sm_id').value = item.id;
            document.getElementById('sm_no_agenda').value = item.no_agenda;
            document.getElementById('sm_no_surat').value = item.no_surat;
            document.getElementById('sm_tgl_surat').value = item.tgl_surat;
            document.getElementById('sm_tgl_terima').value = item.tgl_terima;
            document.getElementById('sm_pengirim').value = item.pengirim;
            document.getElementById('sm_perihal').value = item.perihal;
            document.getElementById('sm_tahun_ajaran').value = item.tahun_ajaran || '2026/2027';
            document.getElementById('sm_file_url').value = item.file_url || '';
            document.getElementById('modalSuratMasuk').classList.remove('hidden');
        }
        function deleteSuratMasuk(id) {
            if (confirm('Yakin ingin menghapus surat masuk ini?')) {
                suratMasukData = suratMasukData.filter(s => s.id !== id);
                localStorage.setItem('sdn_krob_sm', JSON.stringify(suratMasukData));
                renderSuratMasuk();
                updateDashboardStats();
                renderRecentActivity();
            }
        }

        // Modal Surat Keluar
        function openModalSuratKeluar() {
            document.getElementById('modalSuratKeluarTitle').textContent = 'Tambah Surat Keluar';
            document.getElementById('formSuratKeluar').reset();
            document.getElementById('sk_id').value = '';
            document.getElementById('modalSuratKeluar').classList.remove('hidden');
        }
        function closeModalSuratKeluar() {
            document.getElementById('modalSuratKeluar').classList.add('hidden');
        }
        function saveSuratKeluar(e) {
            e.preventDefault();
            const id = document.getElementById('sk_id').value;
            const newData = {
                id: id || 'SK-' + Date.now(),
                no_agenda: document.getElementById('sk_no_agenda').value,
                no_surat: document.getElementById('sk_no_surat').value,
                tgl_surat: document.getElementById('sk_tgl_surat').value,
                tujuan: document.getElementById('sk_tujuan').value,
                perihal: document.getElementById('sk_perihal').value,
                tahun_ajaran: document.getElementById('sk_tahun_ajaran').value,
                file_url: document.getElementById('sk_file_url').value
            };

            if (id) {
                const idx = suratKeluarData.findIndex(s => s.id === id);
                if (idx !== -1) suratKeluarData[idx] = newData;
            } else {
                suratKeluarData.unshift(newData);
            }

            localStorage.setItem('sdn_krob_sk', JSON.stringify(suratKeluarData));
            closeModalSuratKeluar();
            renderSuratKeluar();
            updateDashboardStats();
            renderRecentActivity();
        }
        function editSuratKeluar(id) {
            const item = suratKeluarData.find(s => s.id === id);
            if (!item) return;
            document.getElementById('modalSuratKeluarTitle').textContent = 'Edit Surat Keluar';
            document.getElementById('sk_id').value = item.id;
            document.getElementById('sk_no_agenda').value = item.no_agenda;
            document.getElementById('sk_no_surat').value = item.no_surat;
            document.getElementById('sk_tgl_surat').value = item.tgl_surat;
            document.getElementById('sk_tujuan').value = item.tujuan;
            document.getElementById('sk_perihal').value = item.perihal;
            document.getElementById('sk_tahun_ajaran').value = item.tahun_ajaran || '2026/2027';
            document.getElementById('sk_file_url').value = item.file_url || '';
            document.getElementById('modalSuratKeluar').classList.remove('hidden');
        }
        function deleteSuratKeluar(id) {
            if (confirm('Yakin ingin menghapus surat keluar ini?')) {
                suratKeluarData = suratKeluarData.filter(s => s.id !== id);
                localStorage.setItem('sdn_krob_sk', JSON.stringify(suratKeluarData));
                renderSuratKeluar();
                updateDashboardStats();
                renderRecentActivity();
            }
        }
    </script>
</body>
</html>
