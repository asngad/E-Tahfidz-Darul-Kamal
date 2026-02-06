<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>E-Tahfidz PP Darul Kamal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Amiri:wght@400;700&family=Plus+Jakarta+Sans:wght@300;400;600;700&display=swap" rel="stylesheet">
    
    <style>
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #f3f4f6; }
        .font-arab { font-family: 'Amiri', serif; }
        .bg-islamic {
            background-color: #064E3B;
            background-image: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23065f46' fill-opacity='0.4'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
        }
        .tab-active { border-bottom: 3px solid #059669; color: #059669; font-weight: 700; background-color: #ECFDF5; }
        .tab-inactive { color: #6B7280; border-bottom: 3px solid transparent; }
    </style>
</head>
<body class="bg-gray-50 text-gray-800 antialiased h-screen overflow-hidden">

    <!-- 1. LOGIN SCREEN -->
    <div id="login-screen" class="fixed inset-0 z-50 bg-islamic flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl shadow-2xl w-full max-w-md overflow-hidden relative">
            
            <!-- HEADER -->
            <div class="bg-emerald-800 p-6 text-center relative">
                <div class="absolute inset-0 opacity-10 bg-[url('https://www.transparenttextures.com/patterns/arabesque.png')]"></div>
                
                <!-- Tombol Bantuan / Panduan -->
                <button onclick="togglePanduan()" class="absolute top-4 right-4 text-emerald-200 hover:text-white transition">
                    <i class="fas fa-question-circle text-xl"></i>
                </button>

                <h1 class="font-arab text-3xl text-yellow-400 font-bold mb-1">معهد دار الكمال</h1>
                <p class="text-emerald-100 text-[10px] tracking-[0.2em] uppercase font-semibold">Sistem Informasi Tahfidz</p>
            </div>
            
            <!-- TABS -->
            <div class="flex border-b border-gray-200 relative">
                <button onclick="setRole('santri')" id="tab-santri" class="flex-1 py-3 text-sm transition tab-active"><i class="fas fa-user-graduate mr-1"></i> Santri</button>
                <button onclick="setRole('admin')" id="tab-admin" class="flex-1 py-3 text-sm transition tab-inactive"><i class="fas fa-user-shield mr-1"></i> Admin</button>
                <button onclick="toggleExtraMenu()" class="px-4 py-3 text-gray-400 hover:text-emerald-600 transition hover:bg-gray-50 border-l border-gray-100"><i class="fas fa-ellipsis-v"></i></button>

                <!-- MENU EKSTRA (Wali/Ustadz) -->
                <div id="extra-menu" class="hidden absolute top-full right-0 w-48 bg-white shadow-xl rounded-bl-xl border border-gray-100 z-50 overflow-hidden">
                    <div class="p-2 text-[10px] font-bold text-gray-400 uppercase tracking-wider bg-gray-50 border-b">Akses Lainnya</div>
                    <button onclick="setRole('wali'); toggleExtraMenu()" class="w-full text-left px-4 py-3 hover:bg-emerald-50 text-sm text-gray-700 flex items-center gap-2 border-b border-gray-50"><i class="fas fa-user-friends text-emerald-600 w-5"></i> Wali Santri</button>
                    <button onclick="setRole('ustadz'); toggleExtraMenu()" class="w-full text-left px-4 py-3 hover:bg-emerald-50 text-sm text-gray-700 flex items-center gap-2 border-b border-gray-50"><i class="fas fa-user-tie text-emerald-600 w-5"></i> Ustadz</button>
                    <button onclick="setRole('ustadzah'); toggleExtraMenu()" class="w-full text-left px-4 py-3 hover:bg-emerald-50 text-sm text-gray-700 flex items-center gap-2"><i class="fas fa-user-nurse text-emerald-600 w-5"></i> Ustadzah</button>
                </div>
            </div>

            <!-- INDIKATOR PERAN (Wali/Ustadz) -->
            <div id="role-indicator" class="hidden bg-yellow-50 text-yellow-800 text-xs px-6 py-2 border-b border-yellow-100 flex justify-between items-center">
                <span class="font-bold"><i class="fas fa-info-circle mr-1"></i> Login: <span id="role-label-text">...</span></span>
                <button onclick="setRole('santri')" class="text-yellow-600 hover:text-yellow-900"><i class="fas fa-times"></i></button>
            </div>

            <!-- FORM LOGIN -->
            <div class="p-8 pt-6">
                <form id="loginForm" onsubmit="handleLogin(event)">
                    <div class="space-y-4">
                        <div class="relative">
                            <span class="absolute left-3 top-3.5 text-gray-400"><i id="icon-identity" class="fas fa-id-card"></i></span>
                            <input type="text" id="loginId" class="w-full pl-10 pr-4 py-3 rounded-lg border border-gray-300 focus:border-emerald-500 focus:ring-1 focus:ring-emerald-500 outline-none transition" placeholder="Nomor Induk Santri (NIS)" required>
                        </div>
                        <div class="relative">
                            <span class="absolute left-3 top-3.5 text-gray-400"><i class="fas fa-lock"></i></span>
                            <input type="password" id="loginPass" class="w-full pl-10 pr-4 py-3 rounded-lg border border-gray-300 focus:border-emerald-500 focus:ring-1 focus:ring-emerald-500 outline-none transition" placeholder="Kata Sandi" required>
                        </div>
                    </div>
                    <input type="hidden" id="loginRole" value="santri">
                    <button type="submit" class="w-full mt-6 bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-3 rounded-lg shadow-lg transform active:scale-95 transition-all flex justify-center items-center gap-2"><span>Masuk Aplikasi</span> <i class="fas fa-arrow-right"></i></button>
                </form>
            </div>
        </div>
    </div>

    <!-- MODAL PANDUAN LOGIN -->
    <div id="modal-panduan" class="hidden fixed inset-0 z-[60] bg-black bg-opacity-50 flex items-center justify-center p-4">
        <div class="bg-white rounded-xl shadow-2xl w-full max-w-sm overflow-hidden transform transition-all scale-100">
            <div class="bg-gray-50 px-5 py-4 border-b flex justify-between items-center">
                <h3 class="font-bold text-gray-700"><i class="fas fa-book-reader text-emerald-600 mr-2"></i>Panduan Login</h3>
                <button onclick="togglePanduan()" class="text-gray-400 hover:text-red-500"><i class="fas fa-times"></i></button>
            </div>
            <div class="p-5 text-sm">
                <p class="mb-3 text-gray-500">Gunakan format berikut sesuai peran Anda:</p>
                
                <div class="space-y-3">
                    <div class="border rounded-lg p-3 bg-blue-50 border-blue-100">
                        <div class="font-bold text-blue-800 flex items-center gap-2"><i class="fas fa-user-graduate"></i> Santri</div>
                        <div class="text-xs text-blue-600 mt-1 grid grid-cols-3 gap-1">
                            <span>ID:</span> <span class="col-span-2 font-mono bg-white px-1 rounded border">NIS (Cth: 12345)</span>
                            <span>Pass:</span> <span class="col-span-2 font-mono bg-white px-1 rounded border">******</span>
                        </div>
                    </div>

                    <div class="border rounded-lg p-3 bg-purple-50 border-purple-100">
                        <div class="font-bold text-purple-800 flex items-center gap-2"><i class="fas fa-user-friends"></i> Wali Santri</div>
                        <div class="text-xs text-purple-600 mt-1 grid grid-cols-3 gap-1">
                            <span>ID:</span> <span class="col-span-2 font-mono bg-white px-1 rounded border">No. HP (0812...)</span>
                            <span>Pass:</span> <span class="col-span-2 font-mono bg-white px-1 rounded border">******</span>
                        </div>
                    </div>

                    <div class="border rounded-lg p-3 bg-emerald-50 border-emerald-100">
                        <div class="font-bold text-emerald-800 flex items-center gap-2"><i class="fas fa-user-tie"></i> Ustadz/Admin</div>
                        <div class="text-xs text-emerald-600 mt-1 grid grid-cols-3 gap-1">
                            <span>ID:</span> <span class="col-span-2 font-mono bg-white px-1 rounded border">NIP / Username</span>
                            <span>Pass:</span> <span class="col-span-2 font-mono bg-white px-1 rounded border">******</span>
                        </div>
                    </div>
                </div>
                
                <div class="mt-4 pt-4 border-t text-center">
                    <button onclick="demoLogin()" class="text-xs text-emerald-600 font-bold hover:underline">
                        <i class="fas fa-play-circle mr-1"></i> Coba Akun Demo (Admin)
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- 2. MAIN APP LAYOUT (Sama seperti sebelumnya) -->
    <div id="main-app" class="hidden h-screen flex flex-col md:flex-row">
        <!-- Sidebar -->
        <aside class="bg-emerald-900 text-white w-64 hidden md:flex flex-col shadow-xl z-20">
            <div class="p-6 border-b border-emerald-800 flex items-center gap-3 bg-emerald-950">
                <div class="w-10 h-10 rounded-full bg-yellow-500 flex items-center justify-center text-emerald-900 font-bold font-arab text-xl shadow-lg">د</div>
                <div><h1 class="font-bold text-base leading-tight">Darul Kamal</h1><p class="text-[10px] text-emerald-400 uppercase">E-Tahfidz System</p></div>
            </div>
            <nav class="flex-1 p-4 space-y-2">
                <button onclick="navTo('dashboard')" class="nav-btn w-full text-left px-4 py-3 rounded-lg bg-emerald-800 text-white flex items-center gap-3 transition-all"><i class="fas fa-chart-pie w-5"></i> Dashboard</button>
                <button onclick="navTo('input')" id="menu-input" class="nav-btn w-full text-left px-4 py-3 rounded-lg text-emerald-100 hover:bg-emerald-800 flex items-center gap-3 transition-all"><i class="fas fa-pen-nib w-5"></i> Input Setoran</button>
                <button onclick="navTo('riwayat')" class="nav-btn w-full text-left px-4 py-3 rounded-lg text-emerald-100 hover:bg-emerald-800 flex items-center gap-3 transition-all"><i class="fas fa-history w-5"></i> Riwayat</button>
            </nav>
            <div class="p-4 bg-emerald-950"><button onclick="location.reload()" class="w-full py-2 px-4 rounded border border-emerald-700 text-emerald-300 hover:bg-emerald-800 text-sm transition"><i class="fas fa-sign-out-alt mr-2"></i> Keluar</button></div>
        </aside>

        <!-- Mobile Header -->
        <header class="md:hidden bg-emerald-900 text-white p-4 flex justify-between items-center shadow-md z-30">
            <span class="font-arab text-xl font-bold">دار الكمال</span>
            <button onclick="document.getElementById('mobile-menu').classList.remove('translate-x-full')" class="text-white"><i class="fas fa-bars text-xl"></i></button>
        </header>

        <!-- Mobile Menu Overlay -->
        <div id="mobile-menu" class="fixed inset-0 bg-emerald-900 z-50 transform translate-x-full transition-transform duration-300 md:hidden flex flex-col pt-20 px-6">
            <button onclick="document.getElementById('mobile-menu').classList.add('translate-x-full')" class="absolute top-5 right-5 text-white text-2xl"><i class="fas fa-times"></i></button>
            <button onclick="navTo('dashboard'); closeMobile()" class="py-4 border-b border-emerald-800 text-white text-lg">Dashboard</button>
            <button onclick="navTo('input'); closeMobile()" id="mobile-menu-input" class="py-4 border-b border-emerald-800 text-white text-lg">Input Hafalan</button>
            <button onclick="navTo('riwayat'); closeMobile()" class="py-4 border-b border-emerald-800 text-white text-lg">Riwayat</button>
            <button onclick="location.reload()" class="py-4 text-red-300 mt-auto mb-10">Keluar</button>
        </div>

        <!-- Content Area -->
        <main class="flex-1 p-4 md:p-8 overflow-y-auto h-full relative bg-gray-50">
            <div class="flex justify-between items-end mb-8">
                <div><h2 id="page-title" class="text-2xl font-bold text-emerald-900">Dashboard</h2><p class="text-sm text-gray-500" id="current-date"></p></div>
                <div class="text-right hidden sm:block">
                    <p class="text-sm font-bold text-gray-800" id="user-display-name">...</p>
                    <div class="flex items-center justify-end gap-2 mt-1"><span id="role-badge" class="text-[10px] uppercase font-bold px-2 py-0.5 rounded bg-gray-200 text-gray-600">SANTRI</span></div>
                </div>
            </div>

            <!-- PAGE: DASHBOARD -->
            <div id="page-dashboard" class="page-section space-y-6">
                <div class="bg-gradient-to-r from-emerald-800 to-emerald-600 rounded-xl p-6 text-white shadow-lg relative overflow-hidden">
                    <i class="fas fa-quote-right absolute right-4 bottom-4 text-emerald-500 text-6xl opacity-20"></i>
                    <p class="font-arab text-2xl mb-2 leading-loose">خَيْرُكُمْ مَنْ تَعَلَّمَ الْقُرْآنَ وَعَلَّمَهُ</p>
                    <p class="text-sm opacity-90">"Sebaik-baik kalian adalah yang belajar Al-Qur’an dan mengajarkannya."</p>
                </div>
                <div id="dashboard-alert" class="hidden bg-blue-50 border-l-4 border-blue-500 p-4 rounded shadow-sm">
                    <p class="text-blue-700 text-sm font-bold">Mode Pengawasan</p>
                    <p class="text-blue-600 text-xs">Anda sedang melihat data rekapitulasi hafalan santri.</p>
                </div>
            </div>

            <!-- PAGE: INPUT -->
            <div id="page-input" class="page-section hidden">
                <div class="bg-white rounded-xl shadow-sm p-6 max-w-2xl mx-auto border-t-4 border-emerald-600">
                    <h3 class="font-bold text-gray-800 mb-6 flex items-center gap-2 border-b pb-4"><i class="fas fa-pen-nib text-emerald-600"></i> Form Setoran</h3>
                    <form id="hafalanForm" onsubmit="submitHafalan(event)">
                        <div id="manual-input-area" class="hidden bg-emerald-50 p-4 rounded-lg border border-emerald-200 mb-4">
                            <label class="text-xs font-bold text-emerald-800 uppercase block mb-2"><i class="fas fa-user-edit mr-1"></i> Identitas Santri</label>
                            <input type="text" id="inputNisManual" onchange="document.getElementById('formNis').value = this.value" class="w-full p-2 border rounded text-sm mb-2" placeholder="Masukkan NIS Santri">
                            <input type="text" id="inputNamaManual" onchange="document.getElementById('formNama').value = this.value" class="w-full p-2 border rounded text-sm" placeholder="Nama Santri">
                        </div>
                        <input type="hidden" id="formNis"><input type="hidden" id="formNama">

                        <div class="grid grid-cols-2 gap-4 mb-4">
                            <div><label class="text-xs font-bold text-gray-500 uppercase">Jenis</label><select id="jenis" class="w-full mt-1 p-3 border rounded-lg bg-gray-50 outline-none focus:border-emerald-500"><option value="Ziyadah">Ziyadah</option><option value="Murajaah">Murajaah</option></select></div>
                            <div><label class="text-xs font-bold text-gray-500 uppercase">Surat</label><select id="surat" class="w-full mt-1 p-3 border rounded-lg bg-gray-50 outline-none focus:border-emerald-500"><option value="An-Naba">An-Naba</option><option value="An-Naziat">An-Naziat</option><option value="Abasa">Abasa</option></select></div>
                        </div>
                        <div class="grid grid-cols-2 gap-4 mb-4">
                            <div><label class="text-xs font-bold text-gray-500 uppercase">Ayat Awal</label><input type="number" id="ayatAwal" class="w-full mt-1 p-3 border rounded-lg outline-none focus:border-emerald-500" placeholder="1"></div>
                            <div><label class="text-xs font-bold text-gray-500 uppercase">Ayat Akhir</label><input type="number" id="ayatAkhir" class="w-full mt-1 p-3 border rounded-lg outline-none focus:border-emerald-500" placeholder="10"></div>
                        </div>
                        <div class="mb-4">
                            <label class="text-xs font-bold text-gray-500 uppercase mb-2 block">Predikat</label>
                            <div class="grid grid-cols-3 gap-3">
                                <label class="cursor-pointer text-center"><input type="radio" name="predikat" value="Mumtaz" class="peer sr-only" checked><div class="p-2 border rounded peer-checked:bg-emerald-100 peer-checked:border-emerald-500 peer-checked:text-emerald-700 text-sm font-bold">Mumtaz</div></label>
                                <label class="cursor-pointer text-center"><input type="radio" name="predikat" value="Jayyid" class="peer sr-only"><div class="p-2 border rounded peer-checked:bg-yellow-100 peer-checked:border-yellow-500 peer-checked:text-yellow-700 text-sm font-bold">Jayyid</div></label>
                                <label class="cursor-pointer text-center"><input type="radio" name="predikat" value="Dhaif" class="peer sr-only"><div class="p-2 border rounded peer-checked:bg-red-100 peer-checked:border-red-500 peer-checked:text-red-700 text-sm font-bold">Dhaif</div></label>
                            </div>
                        </div>
                        <div class="mb-6"><label class="text-xs font-bold text-gray-500 uppercase">Catatan</label><textarea id="catatan" rows="2" class="w-full mt-1 p-3 border rounded-lg outline-none focus:border-emerald-500" placeholder="..."></textarea></div>
                        <button type="submit" id="btnSubmit" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-3 rounded-lg shadow-md transition flex justify-center items-center gap-2"><i class="fas fa-paper-plane"></i> Kirim Data</button>
                    </form>
                </div>
            </div>

            <!-- PAGE: RIWAYAT -->
            <div id="page-riwayat" class="page-section hidden">
                <div class="bg-white rounded-xl shadow-sm overflow-hidden">
                    <div class="p-5 border-b flex justify-between items-center bg-gray-50">
                        <h3 class="font-bold text-gray-700" id="riwayat-title">Data Hafalan</h3>
                        <button onclick="loadRiwayat()" class="text-xs text-emerald-600 hover:underline"><i class="fas fa-sync"></i> Refresh</button>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-sm text-gray-600">
                            <thead class="bg-gray-100 uppercase text-xs font-bold text-gray-500">
                                <tr>
                                    <th class="px-6 py-3">Tanggal / Nama</th>
                                    <th class="px-6 py-3">Setoran</th>
                                    <th class="px-6 py-3">Bukti PDF</th>
                                </tr>
                            </thead>
                            <tbody id="riwayatBody" class="divide-y divide-gray-100"><tr><td colspan="3" class="text-center py-4 text-gray-400">Memuat data...</td></tr></tbody>
                        </table>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <!-- TOAST & MODAL SUKSES -->
    <div id="toast" class="fixed bottom-5 right-5 transform translate-y-20 opacity-0 transition-all duration-300 z-50 bg-gray-800 text-white px-6 py-3 rounded-lg shadow-xl flex items-center gap-3"><i class="fas fa-check-circle text-emerald-400 text-xl"></i><span id="toast-msg" class="font-medium">Berhasil</span></div>

    <!-- Script Logic -->
    <script>
        let currentUser = { id: '', role: 'santri', nama: '' };
        document.getElementById('current-date').textContent = new Date().toLocaleDateString('id-ID', { weekday: 'long', day: 'numeric', month: 'long', year: 'numeric' });

        // --- NEW: TOGGLE PANDUAN ---
        function togglePanduan() {
            const modal = document.getElementById('modal-panduan');
            modal.classList.toggle('hidden');
        }

        function toggleExtraMenu() { document.getElementById('extra-menu').classList.toggle('hidden'); }
        
        function setRole(role) {
            document.getElementById('tab-santri').className = "flex-1 py-3 text-sm transition tab-inactive";
            document.getElementById('tab-admin').className = "flex-1 py-3 text-sm transition tab-inactive";
            document.getElementById('role-indicator').classList.add('hidden');
            document.getElementById('loginRole').value = role;
            const inputId = document.getElementById('loginId');
            const iconId = document.getElementById('icon-identity');

            if (role === 'santri') {
                document.getElementById('tab-santri').className = "flex-1 py-3 text-sm transition tab-active";
                inputId.placeholder = "Nomor Induk Santri (NIS)"; iconId.className = "fas fa-id-card";
            } else if (role === 'admin') {
                document.getElementById('tab-admin').className = "flex-1 py-3 text-sm transition tab-active";
                inputId.placeholder = "Username Admin"; iconId.className = "fas fa-user-shield";
            } else {
                document.getElementById('role-indicator').classList.remove('hidden');
                document.getElementById('role-label-text').textContent = role.charAt(0).toUpperCase() + role.slice(1);
                if (role === 'wali') { inputId.placeholder = "Nomor Handphone Wali"; iconId.className = "fas fa-phone"; } 
                else { inputId.placeholder = "ID / NIP Ustadz"; iconId.className = "fas fa-id-badge"; }
            }
        }

        function handleLogin(e) {
            e.preventDefault();
            const btn = e.target.querySelector('button[type="submit"]');
            const originalText = btn.innerHTML;
            btn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Memuat...'; btn.disabled = true;
            const id = document.getElementById('loginId').value;
            const pass = document.getElementById('loginPass').value;
            const role = document.getElementById('loginRole').value;

            google.script.run
                .withSuccessHandler(res => {
                    if (res.status === 'success') loginSuccess(res);
                    else { alert(res.message); btn.innerHTML = originalText; btn.disabled = false; }
                })
                .withFailureHandler(err => { alert("Gagal koneksi: " + err); btn.innerHTML = originalText; btn.disabled = false; })
                .loginSystem(id, pass, role);
        }

        function demoLogin() {
            togglePanduan(); // Tutup panduan
            // Login sebagai admin dummy
            loginSuccess({ status: 'success', role: 'admin', nama: 'Admin Demo (Mode)', id: 'admin' });
        }

        function loginSuccess(data) {
            currentUser = data;
            document.getElementById('login-screen').classList.add('hidden');
            document.getElementById('main-app').classList.remove('hidden');
            document.getElementById('user-display-name').textContent = data.nama;
            document.getElementById('role-badge').textContent = data.role.toUpperCase();

            const isViewer = (data.role === 'wali');
            const isAdmin = (data.role === 'admin' || data.role === 'ustadz' || data.role === 'ustadzah');

            if (isViewer) {
                document.getElementById('menu-input').classList.add('hidden');
                document.getElementById('mobile-menu-input').classList.add('hidden');
                document.getElementById('dashboard-alert').classList.remove('hidden');
                navTo('riwayat');
            } else { navTo('dashboard'); }

            if (isAdmin) {
                document.getElementById('manual-input-area').classList.remove('hidden');
                document.getElementById('formNis').value = ''; document.getElementById('formNama').value = '';
            } else if (data.role === 'santri') {
                document.getElementById('manual-input-area').classList.add('hidden');
                document.getElementById('formNis').value = data.id; document.getElementById('formNama').value = data.nama;
            }
        }

        function navTo(pageId) {
            document.querySelectorAll('.page-section').forEach(el => el.classList.add('hidden'));
            document.getElementById('page-' + pageId).classList.remove('hidden');
            const titles = { 'dashboard': 'Dashboard', 'input': 'Input Setoran', 'riwayat': 'Riwayat Hafalan' };
            document.getElementById('page-title').textContent = titles[pageId];
            document.getElementById('mobile-menu').classList.add('translate-x-full');
            if (pageId === 'riwayat') loadRiwayat();
        }

        function submitHafalan(e) {
            e.preventDefault();
            const btn = document.getElementById('btnSubmit');
            if (!document.getElementById('formNis').value) { alert("NIS Santri wajib diisi!"); return; }
            btn.innerHTML = '<i class="fas fa-cog fa-spin"></i> Generate PDF & Save...'; btn.disabled = true;

            const form = {
                nis: document.getElementById('formNis').value,
                nama: document.getElementById('formNama').value,
                jenis: document.getElementById('jenis').value,
                surat: document.getElementById('surat').value,
                ayatAwal: document.getElementById('ayatAwal').value,
                ayatAkhir: document.getElementById('ayatAkhir').value,
                predikat: document.querySelector('input[name="predikat"]:checked').value,
                catatan: document.getElementById('catatan').value,
                inputBy: currentUser.role
            };

            google.script.run
                .withSuccessHandler(res => {
                    if(res.status === 'success'){
                        showToast("Data Tersimpan! PDF Dibuat.");
                        if(res.pdfUrl && confirm("Laporan PDF berhasil dibuat. Buka sekarang?")) {
                            window.open(res.pdfUrl, '_blank');
                        }
                        document.getElementById('hafalanForm').reset();
                        if(currentUser.role === 'santri') {
                            document.getElementById('formNis').value = currentUser.id; document.getElementById('formNama').value = currentUser.nama;
                        } else {
                            document.getElementById('formNis').value = ''; document.getElementById('formNama').value = '';
                            document.getElementById('inputNisManual').value = ''; document.getElementById('inputNamaManual').value = '';
                        }
                        navTo('riwayat');
                    } else { alert("Gagal: " + res.message); }
                    btn.innerHTML = '<i class="fas fa-paper-plane"></i> Kirim Data'; btn.disabled = false;
                })
                .withFailureHandler(err => { alert(err); btn.disabled = false; })
                .simpanHafalan(form);
        }

        function loadRiwayat() {
            const tbody = document.getElementById('riwayatBody');
            tbody.innerHTML = '<tr><td colspan="3" class="text-center py-4"><i class="fas fa-spinner fa-spin text-emerald-600"></i></td></tr>';
            let title = "Riwayat Saya";
            if (currentUser.role === 'wali') title = "Riwayat Hafalan Anak";
            if (currentUser.role === 'admin' || currentUser.role.includes('ustadz')) title = "Rekapitulasi Santri";
            document.getElementById('riwayat-title').textContent = title;

            google.script.run
                .withSuccessHandler(data => {
                    tbody.innerHTML = '';
                    if (!data || data.length === 0) { tbody.innerHTML = '<tr><td colspan="3" class="text-center py-4 text-gray-400">Belum ada data.</td></tr>'; return; }
                    data.forEach(row => {
                        const tr = document.createElement('tr');
                        tr.className = "hover:bg-gray-50 transition border-b border-gray-100";
                        let col1Content = `<div class="font-bold text-gray-800">${row.tanggal}</div>`;
                        if (currentUser.role === 'admin' || currentUser.role.includes('ustadz')) { col1Content = `<div class="font-bold text-gray-800">${row.nama_santri}</div><div class="text-xs text-gray-400">${row.tanggal}</div>`; }
                        
                        let pdfBtn = row.pdf_url ? `<a href="${row.pdf_url}" target="_blank" class="text-red-500 hover:text-red-700 font-bold text-xs border border-red-200 bg-red-50 px-2 py-1 rounded"><i class="fas fa-file-pdf"></i> PDF</a>` : `<span class="text-gray-300 text-xs">-</span>`;

                        tr.innerHTML = `<td class="px-6 py-3 whitespace-nowrap">${col1Content}</td><td class="px-6 py-3"><div class="font-bold text-gray-700">${row.surat}</div><div class="text-xs text-gray-500">${row.jenis} | Ayat ${row.ayat}</div></td><td class="px-6 py-3">${pdfBtn}</td>`;
                        tbody.appendChild(tr);
                    });
                })
                .getRiwayat(currentUser.id, currentUser.role);
        }

        function showToast(msg) {
            const t = document.getElementById('toast'); document.getElementById('toast-msg').textContent = msg;
            t.classList.remove('translate-y-20', 'opacity-0'); setTimeout(() => t.classList.add('translate-y-20', 'opacity-0'), 3000);
        }
        function closeMobile() { document.getElementById('mobile-menu').classList.add('translate-x-full'); }
    </script>
</body>
</html>
