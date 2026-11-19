<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PhysicsClash: Kuis Interaktif Momentum & Tumbukan</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Plus Jakarta Sans & JetBrains Mono -->
    <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@500;700&family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap" rel="stylesheet">
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- Canvas Confetti -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    
    <style>
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background: radial-gradient(circle at top left, #4f46e5, #312e81 45%, #1e1b4b 100%);
            min-height: 100vh;
            color: #1e293b;
            overflow-x: hidden;
        }
        .font-mono-math {
            font-family: 'JetBrains Mono', monospace;
        }
        /* Kahoot / Quizizz 3D Button Styles */
        .btn-3d {
            transition: all 0.12s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            top: 0;
        }
        .btn-3d:active:not(:disabled) {
            top: 4px;
            box-shadow: 0 0px 0 0 rgba(0,0,0,0.3) !important;
        }
        .opt-red {
            background-color: #ef4444;
            box-shadow: 0 5px 0 0 #b91c1c;
        }
        .opt-red:hover:not(:disabled) { background-color: #f87171; }
        
        .opt-blue {
            background-color: #3b82f6;
            box-shadow: 0 5px 0 0 #1d4ed8;
        }
        .opt-blue:hover:not(:disabled) { background-color: #60a5fa; }
        
        .opt-yellow {
            background-color: #f59e0b;
            box-shadow: 0 5px 0 0 #b45309;
        }
        .opt-yellow:hover:not(:disabled) { background-color: #fbbf24; }
        
        .opt-green {
            background-color: #10b981;
            box-shadow: 0 5px 0 0 #047857;
        }
        .opt-green:hover:not(:disabled) { background-color: #34d399; }

        .btn-primary-3d {
            background: linear-gradient(135deg, #8b5cf6 0%, #6d28d9 100%);
            box-shadow: 0 5px 0 0 #4c1d95;
        }
        .btn-primary-3d:hover { filter: brightness(1.08); }

        @keyframes pulse-fast {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.06); }
        }
        .animate-pulse-fast {
            animation: pulse-fast 0.8s infinite;
        }
        @keyframes slideUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .animate-slide-up {
            animation: slideUp 0.35s ease-out forwards;
        }
    </style>
</head>
<body class="flex flex-col justify-between selection:bg-indigo-300 selection:text-indigo-900">

    <!-- Top Navbar -->
    <header class="w-full px-4 py-3 md:px-8 flex items-center justify-between bg-white/10 backdrop-blur-md border-b border-white/15 text-white sticky top-0 z-30">
        <div class="flex items-center gap-2.5 cursor-pointer" onclick="confirmGoHome()">
            <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-amber-400 to-pink-500 flex items-center justify-center shadow-lg">
                <i data-lucide="atom" class="w-6 h-6 text-white"></i>
            </div>
            <div>
                <h1 class="font-extrabold text-lg md:text-xl tracking-tight leading-none">Physics<span class="text-amber-300">Clash</span></h1>
                <p class="text-[11px] text-indigo-200 font-medium">Momentum & Tumbukan • Kelas XI</p>
            </div>
        </div>

        <div class="flex items-center gap-2 md:gap-3">
            <button onclick="openTeacherModal()" class="flex items-center gap-2 px-3.5 py-2 rounded-xl bg-white/15 hover:bg-white/25 border border-white/20 text-xs md:text-sm font-bold transition shadow-sm">
                <i data-lucide="clipboard-list" class="w-4 h-4 text-amber-300"></i>
                <span>Rekap Nilai <span class="hidden sm:inline">(Mode Guru)</span></span>
                <span id="badge-total-records" class="bg-amber-400 text-indigo-950 text-[11px] font-extrabold px-2 py-0.5 rounded-full">0</span>
            </button>
        </div>
    </header>

    <!-- MAIN CONTAINER -->
    <main class="flex-grow flex items-center justify-center p-4 md:p-6">

        <!-- ==================== VIEW 1: LANDING / START PAGE ==================== -->
        <section id="view-start" class="w-full max-w-xl bg-white rounded-3xl shadow-2xl overflow-hidden animate-slide-up">
            <div class="bg-gradient-to-r from-indigo-600 via-purple-600 to-pink-600 p-6 md:p-8 text-white text-center relative overflow-hidden">
                <div class="inline-flex items-center gap-1.5 bg-white/20 backdrop-blur-sm px-3 py-1 rounded-full text-xs font-bold mb-3">
                    <i data-lucide="zap" class="w-3.5 h-3.5 text-amber-300"></i>
                    <span>10 SOAL HITUNGAN INTERAKTIF</span>
                </div>
                <h2 class="text-2xl md:text-3xl font-extrabold mb-2">Tantangan Tumbukan Fisika</h2>
                <p class="text-indigo-100 text-sm md:text-base max-w-md mx-auto">
                    Uji kecepatan & ketepatanmu menghitung Lenting Sempurna, Lenting Sebagian, dan Tidak Lenting Sama Sekali!
                </p>
            </div>

            <div class="p-6 md:p-8">
                <!-- Info Pills -->
                <div class="grid grid-cols-3 gap-2.5 mb-6 text-center">
                    <div class="bg-indigo-50 p-3 rounded-2xl border border-indigo-100">
                        <div class="text-indigo-600 font-extrabold text-lg">10</div>
                        <div class="text-[11px] font-semibold text-slate-500 uppercase">Soal Hitungan</div>
                    </div>
                    <div class="bg-amber-50 p-3 rounded-2xl border border-amber-100">
                        <div class="text-amber-600 font-extrabold text-lg">90 dtk</div>
                        <div class="text-[11px] font-semibold text-slate-500 uppercase">Waktu / Soal</div>
                    </div>
                    <div class="bg-emerald-50 p-3 rounded-2xl border border-emerald-100">
                        <div class="text-emerald-600 font-extrabold text-lg">1.000+</div>
                        <div class="text-[11px] font-semibold text-slate-500 uppercase">Poin & Streak</div>
                    </div>
                </div>

                <!-- Form Input Siswa -->
                <form id="start-form" onsubmit="startQuiz(event)" class="space-y-4">
                    <div>
                        <label for="student-name" class="block text-sm font-bold text-slate-700 mb-1.5">Nama Lengkap Siswa <span class="text-red-500">*</span></label>
                        <div class="relative">
                            <i data-lucide="user" class="w-5 h-5 text-slate-400 absolute left-3.5 top-3.5"></i>
                            <input type="text" id="student-name" required placeholder="Contoh: Budi Santoso"
                                class="w-full pl-11 pr-4 py-3 rounded-xl border-2 border-slate-200 focus:border-indigo-600 focus:outline-none font-semibold text-slate-800 transition">
                        </div>
                    </div>

                    <div>
                        <label for="student-class" class="block text-sm font-bold text-slate-700 mb-1.5">Kelas <span class="text-red-500">*</span></label>
                        <div class="relative">
                            <i data-lucide="graduation-cap" class="w-5 h-5 text-slate-400 absolute left-3.5 top-3.5"></i>
                            <input type="text" id="student-class" required placeholder="Contoh: XI IPA 2"
                                class="w-full pl-11 pr-4 py-3 rounded-xl border-2 border-slate-200 focus:border-indigo-600 focus:outline-none font-semibold text-slate-800 transition">
                        </div>
                    </div>

                    <div id="form-error" class="hidden text-xs font-bold text-red-600 bg-red-50 p-3 rounded-xl border border-red-200"></div>

                    <button type="submit" class="w-full py-4 rounded-2xl btn-primary-3d btn-3d text-white font-extrabold text-lg tracking-wide flex items-center justify-center gap-2 mt-2">
                        <span>MULAI KUIS SEKARANG</span>
                        <i data-lucide="play" class="w-5 h-5 fill-current"></i>
                    </button>
                </form>

                <!-- Ringkasan Materi Singkat -->
                <div class="mt-6 pt-5 border-t border-slate-100">
                    <details class="group">
                        <summary class="flex items-center justify-between cursor-pointer text-xs font-bold text-indigo-600 hover:text-indigo-800">
                            <span class="flex items-center gap-1.5"><i data-lucide="book-open" class="w-4 h-4"></i> Lihat Catatan Rumus Cepat Sebelum Mulai</span>
                            <i data-lucide="chevron-down" class="w-4 h-4 group-open:rotate-180 transition-transform"></i>
                        </summary>
                        <div class="mt-3 p-3.5 bg-slate-50 rounded-2xl text-xs text-slate-600 space-y-2 font-mono-math border border-slate-200">
                            <p><strong class="text-indigo-700">1. Kekekalan Momentum:</strong> m₁·v₁ + m₂·v₂ = m₁·v₁' + m₂·v₂'</p>
                            <p><strong class="text-indigo-700">2. Koefisien Restitusi (e):</strong> e = -(v₂' - v₁') / (v₂ - v₁)</p>
                            <p>• <strong>Lenting Sempurna:</strong> e = 1</p>
                            <p>• <strong>Lenting Sebagian:</strong> 0 &lt; e &lt; 1 | Tinggi pantul: e = √(h₂/h₁)</p>
                            <p>• <strong>Tidak Lenting Sama Sekali:</strong> e = 0 (v₁' = v₂' = v')</p>
                        </div>
                    </details>
                </div>
            </div>
        </section>

        <!-- ==================== VIEW 2: QUIZ GAMEPLAY ==================== -->
        <section id="view-quiz" class="hidden w-full max-w-4xl animate-slide-up">
            <!-- Top HUD Bar -->
            <div class="bg-white/95 backdrop-blur-md rounded-2xl p-3.5 md:p-4 mb-4 shadow-xl flex flex-wrap items-center justify-between gap-3">
                <div class="flex items-center gap-3">
                    <div class="bg-indigo-600 text-white font-extrabold px-3.5 py-1.5 rounded-xl text-sm">
                        Soal <span id="hud-q-num">1</span>/10
                    </div>
                    <span id="hud-topic" class="hidden sm:inline-block text-xs font-bold px-3 py-1.5 rounded-xl bg-purple-100 text-purple-800">
                        Lenting Sempurna
                    </span>
                </div>

                <!-- Timer Circle / Pill -->
                <div id="timer-box" class="flex items-center gap-2 px-4 py-1.5 rounded-xl bg-slate-100 border-2 border-slate-200 font-mono-math font-extrabold text-lg text-slate-800">
                    <i data-lucide="timer" class="w-5 h-5 text-indigo-600"></i>
                    <span id="hud-timer">90</span>s
                </div>

                <!-- Score & Streak -->
                <div class="flex items-center gap-3">
                    <div id="streak-badge" class="hidden items-center gap-1 bg-gradient-to-r from-orange-500 to-amber-500 text-white text-xs font-extrabold px-3 py-1.5 rounded-xl shadow">
                        <i data-lucide="flame" class="w-4 h-4 fill-current"></i>
                        <span id="hud-streak">0</span>x Streak
                    </div>
                    <div class="bg-slate-900 text-amber-400 font-mono-math font-extrabold px-4 py-1.5 rounded-xl text-sm md:text-base shadow-inner">
                        <span id="hud-score">0</span> pts
                    </div>
                </div>
            </div>

            <!-- Timer Progress Bar -->
            <div class="w-full h-2.5 bg-white/20 rounded-full overflow-hidden mb-5">
                <div id="timer-bar" class="h-full bg-gradient-to-r from-emerald-400 via-amber-400 to-rose-500 transition-all duration-1000 linear" style="width: 100%"></div>
            </div>

            <!-- Question Card -->
            <div class="bg-white rounded-3xl shadow-2xl p-6 md:p-8 mb-5 border-b-4 border-slate-200">
                <div class="flex items-center justify-between mb-3">
                    <span class="text-xs font-extrabold uppercase tracking-wider text-indigo-600 flex items-center gap-1.5">
                        <i data-lucide="calculator" class="w-4 h-4"></i> Pertanyaan Hitungan
                    </span>
                    <span id="hud-student-info" class="text-xs font-semibold text-slate-400"></span>
                </div>
                <p id="question-text" class="text-lg md:text-xl font-bold text-slate-800 leading-relaxed">
                    Memuat soal...
                </p>
            </div>

            <!-- Answer Options Grid (Kahoot Style 2x2) -->
            <div id="options-grid" class="grid grid-cols-1 md:grid-cols-2 gap-3.5 md:gap-4">
                <!-- Options injected via JS -->
            </div>

            <!-- Instant Feedback Drawer / Card -->
            <div id="feedback-panel" class="hidden mt-5 rounded-3xl p-5 md:p-6 shadow-2xl border-2 animate-slide-up">
                <div class="flex flex-col md:flex-row md:items-center justify-between gap-4">
                    <div class="flex items-start gap-3.5">
                        <div id="feedback-icon" class="w-12 h-12 rounded-2xl flex items-center justify-center shrink-0 text-white shadow-md">
                            <!-- Icon injected -->
                        </div>
                        <div>
                            <div class="flex items-center gap-2 flex-wrap">
                                <h3 id="feedback-title" class="text-xl font-extrabold">Jawaban Benar!</h3>
                                <span id="feedback-points" class="text-xs font-extrabold px-2.5 py-0.5 rounded-full bg-white/80 text-slate-800 shadow-sm">+950 Poin</span>
                            </div>
                            <div class="mt-2 bg-white/90 rounded-2xl p-3.5 text-sm text-slate-700 border border-black/5">
                                <p class="font-bold text-xs uppercase tracking-wider text-indigo-700 mb-1">Pembahasan & Rumus:</p>
                                <p id="feedback-explanation" class="font-mono-math text-xs md:text-sm leading-relaxed"></p>
                            </div>
                        </div>
                    </div>

                    <button id="btn-next-question" onclick="nextQuestion()" class="shrink-0 px-6 py-4 rounded-2xl bg-slate-900 hover:bg-slate-800 text-white font-extrabold text-base flex items-center justify-center gap-2 shadow-lg transition cursor-pointer">
                        <span id="btn-next-text">Soal Berikutnya</span>
                        <i data-lucide="arrow-right" class="w-5 h-5"></i>
                    </button>
                </div>
            </div>
        </section>

        <!-- ==================== VIEW 3: STUDENT RESULT & REVIEW ==================== -->
        <section id="view-result" class="hidden w-full max-w-4xl bg-white rounded-3xl shadow-2xl overflow-hidden animate-slide-up">
            <div class="bg-gradient-to-r from-indigo-600 via-purple-600 to-pink-600 p-6 md:p-8 text-white text-center">
                <div class="inline-flex items-center gap-1.5 bg-white/20 px-3.5 py-1 rounded-full text-xs font-bold mb-2">
                    <i data-lucide="check-circle-2" class="w-4 h-4 text-emerald-300"></i>
                    <span>JAWABAN OTOMATIS TEREKAM KE SISTEM</span>
                </div>
                <h2 class="text-2xl md:text-3xl font-extrabold">Hasil Kuis Fisika</h2>
                <p id="res-student-meta" class="text-indigo-100 font-semibold mt-1">Nama Siswa • Kelas</p>
            </div>

            <div class="p-6 md:p-8">
                <!-- Score Summary Cards -->
                <div class="grid grid-cols-2 md:grid-cols-4 gap-3 md:gap-4 mb-6">
                    <div class="bg-indigo-50 border border-indigo-100 p-4 rounded-2xl text-center">
                        <p class="text-xs font-bold text-indigo-500 uppercase">Total Skor</p>
                        <p id="res-total-score" class="text-2xl md:text-3xl font-extrabold text-indigo-700 font-mono-math mt-1">0</p>
                    </div>
                    <div class="bg-emerald-50 border border-emerald-100 p-4 rounded-2xl text-center">
                        <p class="text-xs font-bold text-emerald-600 uppercase">Nilai (0-100)</p>
                        <p id="res-grade" class="text-2xl md:text-3xl font-extrabold text-emerald-700 font-mono-math mt-1">0</p>
                    </div>
                    <div class="bg-amber-50 border border-amber-100 p-4 rounded-2xl text-center">
                        <p class="text-xs font-bold text-amber-600 uppercase">Benar / Salah</p>
                        <p class="text-xl md:text-2xl font-extrabold text-slate-800 mt-1">
                            <span id="res-correct" class="text-emerald-600">0</span> / <span id="res-wrong" class="text-rose-600">0</span>
                        </p>
                    </div>
                    <div class="bg-purple-50 border border-purple-100 p-4 rounded-2xl text-center">
                        <p class="text-xs font-bold text-purple-600 uppercase">Total Waktu</p>
                        <p id="res-time" class="text-xl md:text-2xl font-extrabold text-purple-700 font-mono-math mt-1">0 dtk</p>
                    </div>
                </div>

                <!-- Action Buttons -->
                <div class="flex flex-wrap items-center justify-center gap-3 mb-8 pb-6 border-b border-slate-100">
                    <button onclick="downloadSingleCSV()" class="px-5 py-3 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white font-bold text-sm flex items-center gap-2 shadow-md transition">
                        <i data-lucide="download" class="w-4 h-4"></i>
                        <span>Download Hasil Saya (CSV)</span>
                    </button>
                    <button onclick="openTeacherModal()" class="px-5 py-3 rounded-xl bg-amber-500 hover:bg-amber-600 text-white font-bold text-sm flex items-center gap-2 shadow-md transition">
                        <i data-lucide="users" class="w-4 h-4"></i>
                        <span>Lihat Peringkat & Rekap Kelas</span>
                    </button>
                    <button onclick="resetToHome()" class="px-5 py-3 rounded-xl bg-slate-100 hover:bg-slate-200 text-slate-700 font-bold text-sm flex items-center gap-2 transition">
                        <i data-lucide="rotate-ccw" class="w-4 h-4"></i>
                        <span>Siswa Baru / Main Lagi</span>
                    </button>
                </div>

                <!-- Detailed Breakdown of 10 Questions -->
                <div>
                    <h3 class="text-lg font-extrabold text-slate-800 mb-4 flex items-center gap-2">
                        <i data-lucide="list-checks" class="w-5 h-5 text-indigo-600"></i>
                        <span>Rincian Jawaban & Pembahasan Lengkap</span>
                    </h3>
                    <div id="result-breakdown-list" class="space-y-4">
                        <!-- Injected via JS -->
                    </div>
                </div>
            </div>
        </section>

    </main>

    <!-- ==================== TEACHER RECAP MODAL ==================== -->
    <div id="teacher-modal" class="hidden fixed inset-0 z-50 bg-slate-900/75 backdrop-blur-sm flex items-center justify-center p-3 md:p-6">
        <div class="bg-white w-full max-w-5xl max-h-[90vh] rounded-3xl shadow-2xl flex flex-col overflow-hidden animate-slide-up">
            <!-- Modal Header -->
            <div class="bg-slate-900 text-white px-6 py-4 flex items-center justify-between">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 rounded-xl bg-amber-400 text-slate-950 flex items-center justify-center font-extrabold">
                        <i data-lucide="database" class="w-5 h-5"></i>
                    </div>
                    <div>
                        <h3 class="font-extrabold text-lg">Rekapitulasi Nilai Siswa (Mode Guru)</h3>
                        <p class="text-xs text-slate-400">Data tersimpan otomatis di browser perangkat ini (localStorage)</p>
                    </div>
                </div>
                <button onclick="closeTeacherModal()" class="p-2 rounded-xl bg-white/10 hover:bg-white/20 text-white transition">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>

            <!-- Modal Toolbar -->
            <div class="p-4 bg-slate-50 border-b border-slate-200 flex flex-wrap items-center justify-between gap-3">
                <div class="text-sm font-bold text-slate-700">
                    Total Peserta Tercatat: <span id="modal-record-count" class="text-indigo-600 font-extrabold">0</span> Siswa
                </div>
                <div class="flex flex-wrap items-center gap-2">
                    <button onclick="exportAllCSV()" class="px-4 py-2 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white text-xs md:text-sm font-bold flex items-center gap-1.5 shadow-sm transition">
                        <i data-lucide="file-spreadsheet" class="w-4 h-4"></i>
                        <span>Export Semua Rekap ke CSV/Excel</span>
                    </button>
                    <button onclick="promptClearData()" class="px-4 py-2 rounded-xl bg-rose-100 hover:bg-rose-200 text-rose-700 text-xs md:text-sm font-bold flex items-center gap-1.5 transition">
                        <i data-lucide="trash-2" class="w-4 h-4"></i>
                        <span>Hapus Semua Data</span>
                    </button>
                </div>
            </div>

            <!-- Confirm Clear Banner (Replaces native confirm()) -->
            <div id="confirm-clear-banner" class="hidden bg-rose-600 text-white px-6 py-3 flex flex-wrap items-center justify-between gap-3">
                <span class="text-sm font-bold flex items-center gap-2">
                    <i data-lucide="alert-triangle" class="w-5 h-5"></i>
                    Yakin ingin menghapus seluruh riwayat nilai siswa secara permanen?
                </span>
                <div class="flex items-center gap-2">
                    <button onclick="executeClearData()" class="px-3.5 py-1.5 rounded-lg bg-white text-rose-700 font-extrabold text-xs hover:bg-rose-50">Ya, Hapus Permanen</button>
                    <button onclick="cancelClearData()" class="px-3.5 py-1.5 rounded-lg bg-rose-800 text-white font-bold text-xs hover:bg-rose-900">Batal</button>
                </div>
            </div>

            <!-- Table Container -->
            <div class="flex-grow overflow-auto p-4 md:p-6">
                <div id="empty-records-msg" class="hidden text-center py-12">
                    <div class="w-16 h-16 bg-slate-100 rounded-full flex items-center justify-center mx-auto mb-3 text-slate-400">
                        <i data-lucide="inbox" class="w-8 h-8"></i>
                    </div>
                    <h4 class="font-bold text-slate-700">Belum Ada Data Jawaban Siswa</h4>
                    <p class="text-xs text-slate-500 mt-1">Siswa yang menyelesaikan kuis akan otomatis muncul di tabel peringkat ini.</p>
                </div>

                <div class="overflow-x-auto">
                    <table id="records-table" class="w-full text-left border-collapse">
                        <thead>
                            <tr class="border-b-2 border-slate-200 text-xs font-extrabold text-slate-500 uppercase bg-slate-50">
                                <th class="py-3 px-3 rounded-tl-xl">Peringkat</th>
                                <th class="py-3 px-3">Nama Siswa</th>
                                <th class="py-3 px-3">Kelas</th>
                                <th class="py-3 px-3">Skor Poin</th>
                                <th class="py-3 px-3">Nilai (100)</th>
                                <th class="py-3 px-3">Benar/Salah</th>
                                <th class="py-3 px-3">Waktu</th>
                                <th class="py-3 px-3">Waktu Selesai</th>
                                <th class="py-3 px-3 rounded-tr-xl">Rincian (1-10)</th>
                            </tr>
                        </thead>
                        <tbody id="records-tbody" class="text-sm divide-y divide-slate-100">
                            <!-- Rows injected via JS -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- Custom Toast Notification -->
    <div id="toast-box" class="hidden fixed bottom-5 right-5 z-50 bg-slate-900 text-white px-4 py-3 rounded-2xl shadow-2xl flex items-center gap-2.5 text-sm font-bold animate-slide-up">
        <i data-lucide="info" class="w-5 h-5 text-amber-400"></i>
        <span id="toast-message">Notifikasi</span>
    </div>

    <!-- Footer -->
    <footer class="text-center text-indigo-200/70 text-xs py-4 px-4">
        Media Evaluasi Interaktif Fisika SMA • Materi Momentum & Tumbukan
    </footer>

    <!-- JAVASCRIPT LOGIC -->
    <script>
        // ==================== 1. BANK 10 SOAL HITUNGAN FISIKA ====================
        const QUIZ_QUESTIONS = [
            {
                id: 1,
                topic: "Tumbukan Lenting Sempurna",
                question: "Bola A bermassa 2 kg bergerak ke kanan dengan kecepatan 6 m/s bertumbukan lenting sempurna (e = 1) dengan bola B bermassa 2 kg yang diam. Berapakah kecepatan bola B setelah tumbukan?",
                options: [
                    "0 m/s",
                    "3 m/s ke kanan",
                    "6 m/s ke kanan",
                    "12 m/s ke kanan"
                ],
                correctIndex: 2,
                explanation: "Pada tumbukan lenting sempurna (e = 1) dengan massa kedua benda sama (m₁ = m₂ = 2 kg), kedua benda akan bertukar kecepatan. Karena v₁ = 6 m/s dan v₂ = 0 m/s, maka v₁' = 0 m/s dan v₂' = 6 m/s ke kanan."
            },
            {
                id: 2,
                topic: "Tidak Lenting Sama Sekali",
                question: "Sebuah peluru bermassa 0,02 kg ditembakkan dengan kecepatan 200 m/s dan bersarang di dalam balok kayu bermassa 1,98 kg yang diam di atas lantai licin. Berapakah kecepatan balok dan peluru setelah tumbukan?",
                options: [
                    "2 m/s",
                    "4 m/s",
                    "10 m/s",
                    "20 m/s"
                ],
                correctIndex: 0,
                explanation: "Tumbukan tidak lenting sama sekali (peluru bersarang): m₁·v₁ + m₂·v₂ = (m₁ + m₂)·v' ⇒ (0,02)(200) + (1,98)(0) = (0,02 + 1,98)·v' ⇒ 4 = 2·v' ⇒ v' = 2 m/s."
            },
            {
                id: 3,
                topic: "Tumbukan Lenting Sempurna",
                question: "Benda P (m = 4 kg) bergerak ke kanan dengan kecepatan 5 m/s menumbuk lenting sempurna benda Q (m = 6 kg) yang bergerak berlawanan arah dengan kelajuan 5 m/s. Kecepatan benda P setelah tumbukan adalah...",
                options: [
                    "7 m/s ke kiri",
                    "7 m/s ke kanan",
                    "3 m/s ke kiri",
                    "5 m/s ke kanan"
                ],
                correctIndex: 0,
                explanation: "Ambil arah kanan (+), kiri (-): v₁ = +5 m/s, v₂ = -5 m/s. Hukum Kekekalan Momentum: 4(5) + 6(-5) = 4v₁' + 6v₂' ⇒ 4v₁' + 6v₂' = -10. Karena e = 1: -(v₂' - v₁') / (-5 - 5) = 1 ⇒ v₂' - v₁' = 10 ⇒ v₂' = v₁' + 10. Substitusi: 4v₁' + 6(v₁' + 10) = -10 ⇒ 10v₁' = -70 ⇒ v₁' = -7 m/s (7 m/s ke kiri)."
            },
            {
                id: 4,
                topic: "Tumbukan Lenting Sebagian",
                question: "Dua bola masing-masing bermassa 2 kg dan 3 kg bergerak searah dengan kecepatan 8 m/s dan 3 m/s. Jika terjadi tumbukan lenting sebagian dengan koefisien restitusi e = 0,6, berapakah kecepatan bola pertama (2 kg) setelah tumbukan?",
                options: [
                    "3,2 m/s",
                    "4,5 m/s",
                    "5,0 m/s",
                    "6,2 m/s"
                ],
                correctIndex: 0,
                explanation: "Momentum: 2(8) + 3(3) = 2v₁' + 3v₂' ⇒ 2v₁' + 3v₂' = 25. Restitusi: e = -(v₂' - v₁') / (3 - 8) = 0,6 ⇒ v₂' - v₁' = 3 ⇒ v₂' = v₁' + 3. Substitusi: 2v₁' + 3(v₁' + 3) = 25 ⇒ 5v₁' = 16 ⇒ v₁' = 3,2 m/s."
            },
            {
                id: 5,
                topic: "Pantulan Bola Jatuh Bebas",
                question: "Sebuah bola dijatuhkan bebas dari ketinggian 100 cm di atas lantai datar. Jika koefisien restitusi antara bola dan lantai adalah e = 0,8, berapakah tinggi pantulan pertama bola tersebut?",
                options: [
                    "80 cm",
                    "64 cm",
                    "50 cm",
                    "36 cm"
                ],
                correctIndex: 1,
                explanation: "Rumus koefisien restitusi bola memantul: e = √(h₂ / h₁) ⇒ e² = h₂ / h₁ ⇒ h₂ = e² × h₁ = (0,8)² × 100 cm = 0,64 × 100 cm = 64 cm."
            },
            {
                id: 6,
                topic: "Menentukan Koefisien Restitusi",
                question: "Bola karet dijatuhkan dari ketinggian 1,8 m dan memantul kembali hingga mencapai ketinggian 0,8 m. Berapakah nilai koefisien restitusi (e) antara bola dan lantai?",
                options: [
                    "0,44",
                    "0,50",
                    "0,67",
                    "0,75"
                ],
                correctIndex: 2,
                explanation: "Gunakan rumus e = √(h₂ / h₁) = √(0,8 / 1,8) = √(8 / 18) = √(4 / 9) = 2/3 ≈ 0,67."
            },
            {
                id: 7,
                topic: "Tidak Lenting Sama Sekali",
                question: "Gerbong kereta A bermassa 4.000 kg bergerak dengan kecepatan 6 m/s menabrak gerbong B bermassa 2.000 kg yang diam. Setelah bertumbukan keduanya saling mengait dan bergerak bersama. Kelajuan kedua gerbong setelah tumbukan adalah...",
                options: [
                    "2 m/s",
                    "3 m/s",
                    "4 m/s",
                    "5 m/s"
                ],
                correctIndex: 2,
                explanation: "Saling mengait berarti tidak lenting sama sekali (e = 0): m_A·v_A + m_B·v_B = (m_A + m_B)·v' ⇒ (4.000)(6) + 0 = (4.000 + 2.000)·v' ⇒ 24.000 = 6.000·v' ⇒ v' = 4 m/s."
            },
            {
                id: 8,
                topic: "Tumbukan Lenting Sebagian",
                question: "Bola X (m = 1 kg) bergerak ke kanan 10 m/s menumbuk bola Y (m = 2 kg) yang bergerak ke kiri 2 m/s. Jika koefisien restitusi e = 0,5, berapakah kecepatan bola Y setelah tumbukan?",
                options: [
                    "4 m/s ke kanan",
                    "2 m/s ke kiri",
                    "6 m/s ke kanan",
                    "4 m/s ke kiri"
                ],
                correctIndex: 0,
                explanation: "v₁ = +10 m/s, v₂ = -2 m/s. Momentum: 1(10) + 2(-2) = 1v₁' + 2v₂' ⇒ v₁' + 2v₂' = 6. Restitusi: 0,5 = -(v₂' - v₁') / (-2 - 10) ⇒ v₂' - v₁' = 6 ⇒ v₁' = v₂' - 6. Substitusi: (v₂' - 6) + 2v₂' = 6 ⇒ 3v₂' = 12 ⇒ v₂' = +4 m/s (4 m/s ke kanan)."
            },
            {
                id: 9,
                topic: "Pantulan Bertingkat",
                question: "Sebuah bola dijatuhkan dari ketinggian 8 m. Jika koefisien restitusi e = 0,5, berapakah tinggi pantulan KEDUA (h₃) bola tersebut?",
                options: [
                    "4,0 m",
                    "2,0 m",
                    "1,0 m",
                    "0,5 m"
                ],
                correctIndex: 3,
                explanation: "Pantulan pertama: h₂ = e² · h₁ = (0,5)² × 8 = 0,25 × 8 = 2 m. Pantulan kedua: h₃ = e² · h₂ = (0,5)² × 2 = 0,25 × 2 = 0,5 m."
            },
            {
                id: 10,
                topic: "Energi Kinetik Hilang (Tidak Lenting)",
                question: "Benda bermassa 2 kg bergerak dengan kecepatan 6 m/s menumbuk secara tidak lenting sama sekali benda bermassa 4 kg yang sedang diam. Berapakah besar energi kinetik yang hilang akibat tumbukan tersebut?",
                options: [
                    "12 Joule",
                    "24 Joule",
                    "36 Joule",
                    "48 Joule"
                ],
                correctIndex: 1,
                explanation: "1) Kecepatan akhir: 2(6) + 0 = (2+4)v' ⇒ v' = 2 m/s. 2) Ek awal = ½·2·6² = 36 J. 3) Ek akhir = ½·(2+4)·2² = 12 J. 4) ΔEk hilang = 36 J - 12 J = 24 Joule."
            }
        ];

        // ==================== 2. STATE MANAGEMENT ====================
        const STORAGE_KEY = "physics_clash_tumbukan_records_v1";
        const TIME_PER_QUESTION = 90; // 90 detik per soal hitungan

        let currentState = {
            studentName: "",
            studentClass: "",
            currentIndex: 0,
            score: 0,
            streak: 0,
            correctCount: 0,
            wrongCount: 0,
            totalTimeSpent: 0,
            questionStartTime: 0,
            timeLeft: TIME_PER_QUESTION,
            timerInterval: null,
            answered: false,
            answersLog: [] // Menyimpan rincian tiap nomor
        };

        let lastSavedRecord = null;

        // ==================== 3. INITIALIZATION & STORAGE ====================
        window.onload = function() {
            lucide.createIcons();
            updateRecordBadge();
        };

        function getRecords() {
            try {
                const raw = localStorage.getItem(STORAGE_KEY);
                return raw ? JSON.parse(raw) : [];
            } catch (e) {
                console.error("Gagal membaca localStorage:", e);
                return [];
            }
        }

        function saveRecord(record) {
            try {
                const records = getRecords();
                records.push(record);
                localStorage.setItem(STORAGE_KEY, JSON.stringify(records));
                updateRecordBadge();
            } catch (e) {
                showToast("Peringatan: Penyimpanan browser penuh, gagal menyimpan otomatis.");
            }
        }

        function updateRecordBadge() {
            const count = getRecords().length;
            document.getElementById("badge-total-records").textContent = count;
        }

        // ==================== 4. QUIZ FLOW FUNCTIONS ====================
        function startQuiz(e) {
            e.preventDefault();
            const nameInput = document.getElementById("student-name").value.trim();
            const classInput = document.getElementById("student-class").value.trim();
            const errorBox = document.getElementById("form-error");

            if (!nameInput || !classInput) {
                errorBox.textContent = "Harap isi Nama Lengkap dan Kelas terlebih dahulu!";
                errorBox.classList.remove("hidden");
                return;
            }
            errorBox.classList.add("hidden");

            // Reset State
            currentState = {
                studentName: nameInput,
                studentClass: classInput,
                currentIndex: 0,
                score: 0,
                streak: 0,
                correctCount: 0,
                wrongCount: 0,
                totalTimeSpent: 0,
                questionStartTime: Date.now(),
                timeLeft: TIME_PER_QUESTION,
                timerInterval: null,
                answered: false,
                answersLog: []
            };

            document.getElementById("hud-student-info").textContent = `${nameInput} (${classInput})`;
            switchView("view-quiz");
            renderQuestion();
        }

        function renderQuestion() {
            const q = QUIZ_QUESTIONS[currentState.currentIndex];
            currentState.answered = false;
            currentState.timeLeft = TIME_PER_QUESTION;
            currentState.questionStartTime = Date.now();

            // Update HUD
            document.getElementById("hud-q-num").textContent = currentState.currentIndex + 1;
            document.getElementById("hud-topic").textContent = q.topic;
            document.getElementById("hud-score").textContent = currentState.score.toLocaleString("id-ID");
            document.getElementById("question-text").textContent = q.question;

            // Update Streak Badge
            const streakBadge = document.getElementById("streak-badge");
            if (currentState.streak >= 2) {
                streakBadge.classList.remove("hidden");
                streakBadge.classList.add("flex");
                document.getElementById("hud-streak").textContent = currentState.streak;
            } else {
                streakBadge.classList.add("hidden");
                streakBadge.classList.remove("flex");
            }

            // Hide Feedback Panel
            document.getElementById("feedback-panel").classList.add("hidden");

            // Render 4 Options
            const optionsGrid = document.getElementById("options-grid");
            optionsGrid.innerHTML = "";

            const styles = [
                { cls: "opt-red", letter: "A", shape: "triangle" },
                { cls: "opt-blue", letter: "B", shape: "diamond" },
                { cls: "opt-yellow", letter: "C", shape: "circle" },
                { cls: "opt-green", letter: "D", shape: "square" }
            ];

            q.options.forEach((optText, idx) => {
                const st = styles[idx];
                const btn = document.createElement("button");
                btn.type = "button";
                btn.id = `opt-btn-${idx}`;
                btn.onclick = () => handleAnswer(idx);
                btn.className = `${st.cls} btn-3d w-full p-4 md:p-5 rounded-2xl text-white font-bold text-left flex items-center gap-4 transition cursor-pointer`;
                btn.innerHTML = `
                    <span class="w-10 h-10 rounded-xl bg-black/20 flex items-center justify-center font-extrabold text-lg shrink-0">
                        ${st.letter}
                    </span>
                    <span class="text-base md:text-lg font-mono-math leading-snug">${optText}</span>
                `;
                optionsGrid.appendChild(btn);
            });

            lucide.createIcons();
            startTimer();
        }

        function startTimer() {
            clearInterval(currentState.timerInterval);
            updateTimerUI();

            currentState.timerInterval = setInterval(() => {
                currentState.timeLeft--;
                updateTimerUI();

                if (currentState.timeLeft <= 0) {
                    clearInterval(currentState.timerInterval);
                    handleTimeUp();
                }
            }, 1000);
        }

        function updateTimerUI() {
            const timerEl = document.getElementById("hud-timer");
            const timerBox = document.getElementById("timer-box");
            const timerBar = document.getElementById("timer-bar");

            timerEl.textContent = currentState.timeLeft;
            const pct = (currentState.timeLeft / TIME_PER_QUESTION) * 100;
            timerBar.style.width = `${pct}%`;

            if (currentState.timeLeft <= 15) {
                timerBox.classList.add("bg-rose-100", "border-rose-400", "text-rose-700", "animate-pulse-fast");
                timerBox.classList.remove("bg-slate-100", "border-slate-200", "text-slate-800");
            } else {
                timerBox.classList.remove("bg-rose-100", "border-rose-400", "text-rose-700", "animate-pulse-fast");
                timerBox.classList.add("bg-slate-100", "border-slate-200", "text-slate-800");
            }
        }

        function handleTimeUp() {
            if (currentState.answered) return;
            handleAnswer(-1); // -1 menandakan waktu habis
        }

        function handleAnswer(selectedIndex) {
            if (currentState.answered) return;
            currentState.answered = true;
            clearInterval(currentState.timerInterval);

            const q = QUIZ_QUESTIONS[currentState.currentIndex];
            const isTimeout = selectedIndex === -1;
            const isCorrect = selectedIndex === q.correctIndex;
            const timeTaken = Math.min(TIME_PER_QUESTION, Math.max(1, TIME_PER_QUESTION - currentState.timeLeft));
            currentState.totalTimeSpent += timeTaken;

            // Hitung Poin (Base 600 + Speed Bonus up to 400 + Streak Bonus)
            let earnedPoints = 0;
            if (isCorrect) {
                currentState.correctCount++;
                currentState.streak++;
                const speedBonus = Math.round((currentState.timeLeft / TIME_PER_QUESTION) * 400);
                const streakBonus = (currentState.streak > 1) ? Math.min(300, (currentState.streak - 1) * 75) : 0;
                earnedPoints = 600 + speedBonus + streakBonus;
                currentState.score += earnedPoints;
            } else {
                currentState.wrongCount++;
                currentState.streak = 0;
            }

            // Update HUD Score
            document.getElementById("hud-score").textContent = currentState.score.toLocaleString("id-ID");

            // Catat Log Jawaban
            const letters = ["A", "B", "C", "D"];
            currentState.answersLog.push({
                questionNumber: q.id,
                topic: q.topic,
                question: q.question,
                selectedOption: isTimeout ? "Waktu Habis" : `${letters[selectedIndex]}. ${q.options[selectedIndex]}`,
                correctOption: `${letters[q.correctIndex]}. ${q.options[q.correctIndex]}`,
                isCorrect: isCorrect,
                points: earnedPoints,
                timeTaken: timeTaken,
                explanation: q.explanation
            });

            // Highlight Pilihan Benar & Salah
            q.options.forEach((_, idx) => {
                const btn = document.getElementById(`opt-btn-${idx}`);
                btn.disabled = true;
                if (idx === q.correctIndex) {
                    btn.classList.add("ring-4", "ring-emerald-300", "scale-[1.02]");
                } else if (idx === selectedIndex) {
                    btn.classList.add("opacity-75", "ring-4", "ring-rose-900");
                } else {
                    btn.classList.add("opacity-40");
                }
            });

            // Tampilkan Panel Umpan Balik
            showFeedbackPanel(isCorrect, isTimeout, earnedPoints, q.explanation);
        }

        function showFeedbackPanel(isCorrect, isTimeout, points, explanation) {
            const panel = document.getElementById("feedback-panel");
            const iconBox = document.getElementById("feedback-icon");
            const titleEl = document.getElementById("feedback-title");
            const ptsEl = document.getElementById("feedback-points");
            const expEl = document.getElementById("feedback-explanation");
            const nextText = document.getElementById("btn-next-text");

            panel.classList.remove("hidden", "bg-emerald-50", "border-emerald-300", "bg-rose-50", "border-rose-300");

            if (isCorrect) {
                panel.classList.add("bg-emerald-50", "border-emerald-300");
                iconBox.className = "w-12 h-12 rounded-2xl flex items-center justify-center shrink-0 text-white shadow-md bg-emerald-600";
                iconBox.innerHTML = `<i data-lucide="check" class="w-7 h-7 stroke-[3]"></i>`;
                titleEl.textContent = currentState.streak >= 3 ? `Luar Biasa! (${currentState.streak}x Streak)` : "Jawaban Tepat!";
                titleEl.className = "text-xl font-extrabold text-emerald-800";
                ptsEl.textContent = `+${points} Poin`;
            } else {
                panel.classList.add("bg-rose-50", "border-rose-300");
                iconBox.className = "w-12 h-12 rounded-2xl flex items-center justify-center shrink-0 text-white shadow-md bg-rose-600";
                iconBox.innerHTML = `<i data-lucide="x" class="w-7 h-7 stroke-[3]"></i>`;
                titleEl.textContent = isTimeout ? "Waktu Habis!" : "Kurang Tepat!";
                titleEl.className = "text-xl font-extrabold text-rose-800";
                ptsEl.textContent = "+0 Poin";
            }

            expEl.textContent = explanation;
            nextText.textContent = (currentState.currentIndex === QUIZ_QUESTIONS.length - 1) ? "Lihat Hasil & Rekap" : "Soal Berikutnya";

            lucide.createIcons();
            panel.scrollIntoView({ behavior: "smooth", block: "nearest" });
        }

        function nextQuestion() {
            if (currentState.currentIndex < QUIZ_QUESTIONS.length - 1) {
                currentState.currentIndex++;
                renderQuestion();
            } else {
                finishQuiz();
            }
        }

        // ==================== 5. FINISH & RECORDING ====================
        function finishQuiz() {
            const grade100 = Math.round((currentState.correctCount / QUIZ_QUESTIONS.length) * 100);
            const now = new Date();
            const timestampStr = now.toLocaleString("id-ID", {
                day: "2-digit", month: "2-digit", year: "numeric",
                hour: "2-digit", minute: "2-digit"
            });

            const record = {
                id: "REC-" + Date.now(),
                name: currentState.studentName,
                className: currentState.studentClass,
                score: currentState.score,
                grade: grade100,
                correct: currentState.correctCount,
                wrong: currentState.wrongCount,
                totalTime: currentState.totalTimeSpent,
                timestamp: timestampStr,
                details: currentState.answersLog
            };

            saveRecord(record);
            lastSavedRecord = record;

            // Render Result Screen
            document.getElementById("res-student-meta").textContent = `${record.name} • Kelas ${record.className}`;
            document.getElementById("res-total-score").textContent = record.score.toLocaleString("id-ID");
            document.getElementById("res-grade").textContent = record.grade;
            document.getElementById("res-correct").textContent = record.correct;
            document.getElementById("res-wrong").textContent = record.wrong;
            document.getElementById("res-time").textContent = `${record.totalTime} dtk`;

            // Render Detailed Breakdown
            const listEl = document.getElementById("result-breakdown-list");
            listEl.innerHTML = "";

            record.details.forEach((item) => {
                const card = document.createElement("div");
                card.className = `p-4 md:p-5 rounded-2xl border-2 ${item.isCorrect ? "bg-emerald-50/50 border-emerald-200" : "bg-rose-50/50 border-rose-200"}`;
                card.innerHTML = `
                    <div class="flex flex-wrap items-center justify-between gap-2 mb-2">
                        <span class="font-extrabold text-sm text-slate-800">Soal #${item.questionNumber} <span class="text-xs font-semibold text-indigo-600 ml-1">(${item.topic})</span></span>
                        <span class="text-xs font-extrabold px-2.5 py-1 rounded-full ${item.isCorrect ? "bg-emerald-600 text-white" : "bg-rose-600 text-white"}">
                            ${item.isCorrect ? `BENAR (+${item.points} pts)` : "SALAH (0 pts)"}
                        </span>
                    </div>
                    <p class="text-sm font-semibold text-slate-700 mb-3">${item.question}</p>
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-2 text-xs mb-3">
                        <div class="p-2.5 rounded-xl bg-white border border-slate-200">
                            <span class="text-slate-400 block font-bold">Jawaban Kamu:</span>
                            <span class="font-bold ${item.isCorrect ? "text-emerald-700" : "text-rose-700"}">${item.selectedOption}</span>
                        </div>
                        <div class="p-2.5 rounded-xl bg-white border border-slate-200">
                            <span class="text-slate-400 block font-bold">Kunci Jawaban:</span>
                            <span class="font-bold text-emerald-700">${item.correctOption}</span>
                        </div>
                    </div>
                    <div class="p-3 rounded-xl bg-indigo-50/80 text-xs font-mono-math text-slate-700 border border-indigo-100">
                        <strong class="text-indigo-800">Pembahasan:</strong> ${item.explanation}
                    </div>
                `;
                listEl.appendChild(card);
            });

            switchView("view-result");
            lucide.createIcons();

            if (grade100 >= 70 && typeof confetti === "function") {
                confetti({ particleCount: 90, spread: 70, origin: { y: 0.6 } });
            }
        }

        // ==================== 6. TEACHER RECAP & CSV EXPORT ====================
        function openTeacherModal() {
            const modal = document.getElementById("teacher-modal");
            const records = getRecords();
            const tbody = document.getElementById("records-tbody");
            const emptyMsg = document.getElementById("empty-records-msg");
            const tableEl = document.getElementById("records-table");

            document.getElementById("modal-record-count").textContent = records.length;
            cancelClearData();

            if (records.length === 0) {
                emptyMsg.classList.remove("hidden");
                tableEl.classList.add("hidden");
            } else {
                emptyMsg.classList.add("hidden");
                tableEl.classList.remove("hidden");

                // Urutkan berdasarkan Skor Tertinggi
                const sorted = [...records].sort((a, b) => b.score - a.score || a.totalTime - b.totalTime);
                tbody.innerHTML = "";

                sorted.forEach((rec, idx) => {
                    const rankBadge = idx === 0 ? "🥇 1" : idx === 1 ? "🥈 2" : idx === 2 ? "🥉 3" : `#${idx + 1}`;
                    const statusPills = rec.details.map(d =>
                        `<span title="Soal ${d.questionNumber}: ${d.isCorrect ? 'Benar' : 'Salah'}" class="inline-block w-4 h-4 rounded text-[10px] font-bold text-center leading-4 text-white ${d.isCorrect ? 'bg-emerald-500' : 'bg-rose-500'}">${d.questionNumber}</span>`
                    ).join(" ");

                    const tr = document.createElement("tr");
                    tr.className = "hover:bg-indigo-50/40 transition";
                    tr.innerHTML = `
                        <td class="py-3 px-3 font-extrabold text-slate-700">${rankBadge}</td>
                        <td class="py-3 px-3 font-bold text-slate-900">${escapeHTML(rec.name)}</td>
                        <td class="py-3 px-3 font-semibold text-slate-600">${escapeHTML(rec.className)}</td>
                        <td class="py-3 px-3 font-mono-math font-extrabold text-indigo-600">${rec.score.toLocaleString("id-ID")}</td>
                        <td class="py-3 px-3 font-mono-math font-extrabold ${rec.grade >= 75 ? 'text-emerald-600' : 'text-amber-600'}">${rec.grade}</td>
                        <td class="py-3 px-3 text-xs font-bold"><span class="text-emerald-600">${rec.correct}B</span> / <span class="text-rose-600">${rec.wrong}S</span></td>
                        <td class="py-3 px-3 font-mono-math text-xs text-slate-600">${rec.totalTime}s</td>
                        <td class="py-3 px-3 text-xs text-slate-500">${rec.timestamp}</td>
                        <td class="py-3 px-3 whitespace-nowrap">${statusPills}</td>
                    `;
                    tbody.appendChild(tr);
                });
            }

            modal.classList.remove("hidden");
            lucide.createIcons();
        }

        function closeTeacherModal() {
            document.getElementById("teacher-modal").classList.add("hidden");
        }

        function promptClearData() {
            if (getRecords().length === 0) {
                showToast("Tidak ada data untuk dihapus.");
                return;
            }
            document.getElementById("confirm-clear-banner").classList.remove("hidden");
        }

        function cancelClearData() {
            document.getElementById("confirm-clear-banner").classList.add("hidden");
        }

        function executeClearData() {
            localStorage.removeItem(STORAGE_KEY);
            updateRecordBadge();
            cancelClearData();
            openTeacherModal();
            showToast("Seluruh data rekap siswa berhasil dihapus.");
        }

        function downloadSingleCSV() {
            if (!lastSavedRecord) return;
            generateCSV([lastSavedRecord], `Hasil_Kuis_Tumbukan_${lastSavedRecord.name.replace(/\s+/g, "_")}.csv`);
        }

        function exportAllCSV() {
            const records = getRecords();
            if (records.length === 0) {
                showToast("Belum ada data siswa untuk diunduh.");
                return;
            }
            const dateTag = new Date().toISOString().slice(0, 10);
            generateCSV(records, `Rekap_Nilai_Kuis_Tumbukan_${dateTag}.csv`);
        }

        function generateCSV(recordsArray, filename) {
            // Header CSV dengan pemisah titik koma/koma standar Excel (menggunakan BOM UTF-8)
            const headers = [
                "No", "Nama Siswa", "Kelas", "Skor Poin", "Nilai (0-100)",
                "Jumlah Benar", "Jumlah Salah", "Total Waktu (detik)", "Waktu Selesai",
                "S1", "S2", "S3", "S4", "S5", "S6", "S7", "S8", "S9", "S10"
            ];

            const rows = recordsArray.map((r, i) => {
                const qResults = [];
                for (let n = 0; n < 10; n++) {
                    const detail = r.details[n];
                    qResults.push(detail ? (detail.isCorrect ? "Benar" : "Salah") : "-");
                }
                return [
                    i + 1,
                    `"${r.name.replace(/"/g, '""')}"`,
                    `"${r.className.replace(/"/g, '""')}"`,
                    r.score,
                    r.grade,
                    r.correct,
                    r.wrong,
                    r.totalTime,
                    `"${r.timestamp}"`,
                    ...qResults
                ].join(",");
            });

            const csvContent = "\uFEFF" + [headers.join(","), ...rows].join("\n");
            const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" });
            const url = URL.createObjectURL(blob);

            const a = document.createElement("a");
            a.href = url;
            a.download = filename;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
            showToast(`File ${filename} berhasil diunduh!`);
        }

        // ==================== 7. NAVIGATION & UTILITIES ====================
        function switchView(viewId) {
            ["view-start", "view-quiz", "view-result"].forEach(id => {
                document.getElementById(id).classList.add("hidden");
            });
            document.getElementById(viewId).classList.remove("hidden");
            window.scrollTo({ top: 0, behavior: "smooth" });
        }

        function resetToHome() {
            clearInterval(currentState.timerInterval);
            document.getElementById("student-name").value = "";
            switchView("view-start");
        }

        function confirmGoHome() {
            const isQuizActive = !document.getElementById("view-quiz").classList.contains("hidden");
            if (isQuizActive) {
                showToast("Selesaikan kuis atau klik 'Siswa Baru' di akhir untuk keluar.");
                return;
            }
            resetToHome();
        }

        function showToast(msg) {
            const box = document.getElementById("toast-box");
            document.getElementById("toast-message").textContent = msg;
            box.classList.remove("hidden");
            clearTimeout(window.toastTimer);
            window.toastTimer = setTimeout(() => {
                box.classList.add("hidden");
            }, 3500);
        }

        function escapeHTML(str) {
            return str.replace(/[&<>'"]/g, tag => ({
                '&': '&amp;', '<': '&lt;', '>': '&gt;', "'": '&#39;', '"': '&quot;'
            }[tag]));
        }
    </script>
</body>
</html>
