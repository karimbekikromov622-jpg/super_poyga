[IQ test.html](https://github.com/user-attachments/files/34005378/IQ.test.html)
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Super Poyga - Bilimlar Musobaqasi</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Canvas Confetti -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@400;600;800;900&display=swap" rel="stylesheet">

    <style>
        body {
            font-family: 'Outfit', sans-serif;
            background: linear-gradient(135deg, #0b0f19 0%, #111827 50%, #1e1b4b 100%);
            color: #ffffff;
            min-height: 100vh;
            overflow-x: hidden;
        }

        .glass-panel {
            background: rgba(17, 24, 39, 0.85);
            backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .glass-card {
            background: rgba(30, 41, 59, 0.7);
            border: 1px solid rgba(255, 255, 255, 0.08);
            backdrop-filter: blur(8px);
        }

        /* SMS Notification Toast Animation */
        @keyframes slideDown {
            0% { transform: translateY(-100%) scale(0.9); opacity: 0; }
            100% { transform: translateY(0) scale(1); opacity: 1; }
        }

        .sms-toast-anim {
            animation: slideDown 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
        }

        /* Nitro glow effect */
        @keyframes pulse-glow {
            0%, 100% { box-shadow: 0 0 20px rgba(245, 158, 11, 0.7); }
            50% { box-shadow: 0 0 40px rgba(239, 68, 68, 0.9); }
        }

        .nitro-active {
            animation: pulse-glow 0.5s infinite alternate;
        }

        /* Road Track background */
        .road-container {
            background: #111827;
            position: relative;
            overflow: hidden;
            border-radius: 1rem;
            border: 2px solid #374151;
        }

        .road-lane {
            position: relative;
            height: 110px;
            background: #1f2937;
            border-bottom: 4px dashed #4b5563;
        }

        .road-finish-line {
            position: absolute;
            right: 20px;
            top: 0;
            bottom: 0;
            width: 24px;
            background: repeating-linear-gradient(
                0deg,
                #fff,
                #fff 10px,
                #000 10px,
                #000 20px
            );
            z-index: 5;
            box-shadow: 0 0 10px rgba(255,255,255,0.5);
        }

        .car-sprite {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            transition: left 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
            z-index: 10;
        }

        /* Exhaust flame effect */
        .exhaust-flame {
            position: absolute;
            left: -24px;
            top: 50%;
            transform: translateY(-50%);
            width: 20px;
            height: 12px;
            background: linear-gradient(90deg, transparent, #f97316, #ef4444);
            border-radius: 50% 0 0 50%;
            opacity: 0;
            transition: opacity 0.3s;
        }

        .car-sprite.moving .exhaust-flame {
            opacity: 1;
            animation: flameFlicker 0.15s infinite alternate;
        }

        @keyframes flameFlicker {
            0% { width: 18px; opacity: 0.8; }
            100% { width: 28px; opacity: 1; }
        }

        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #0b0f19;
        }
        ::-webkit-scrollbar-thumb {
            background: #3b82f6;
            border-radius: 4px;
        }
    </style>
</head>
<body class="flex flex-col min-h-screen justify-between relative">

    <!-- SIMULATED SMS NOTIFICATION TOAST -->
    <div id="smsToast" class="hidden fixed top-4 left-1/2 transform -translate-x-1/2 z-50 w-11/12 max-w-md bg-slate-900/95 text-white p-4 rounded-2xl border-2 border-amber-400 shadow-2xl sms-toast-anim backdrop-blur-xl">
        <div class="flex items-start space-x-3">
            <div class="bg-amber-500/20 p-2.5 rounded-xl text-amber-400">
                <i class="fa-solid fa-comment-sms text-2xl"></i>
            </div>
            <div class="flex-1">
                <div class="flex items-center justify-between">
                    <span class="text-xs font-bold text-amber-400 uppercase tracking-wider">SMS Xabar</span>
                    <span class="text-[10px] text-slate-400">Hozirina</span>
                </div>
                <p class="text-sm font-semibold text-slate-200 mt-0.5">Sizning kirish kodingiz (OTP):</p>
                <div class="flex items-center justify-between mt-1 bg-slate-800/80 px-3 py-1.5 rounded-lg border border-slate-700">
                    <span id="smsCodeDisplay" class="text-2xl font-black tracking-widest text-yellow-400">----</span>
                    <button onclick="copyCode()" class="text-xs text-slate-400 hover:text-white flex items-center gap-1">
                        <i class="fa-regular fa-copy"></i> Nusha
                    </button>
                </div>
            </div>
            <button onclick="hideSmsToast()" class="text-slate-400 hover:text-white">
                <i class="fa-solid fa-xmark text-lg"></i>
            </button>
        </div>
    </div>

    <!-- App Header -->
    <header id="appHeader" class="glass-panel sticky top-0 z-40 px-4 py-3 shadow-2xl border-b border-slate-800 hidden">
        <div class="max-w-6xl mx-auto flex justify-between items-center">
            <div class="flex items-center space-x-3">
                <div class="bg-gradient-to-tr from-red-600 via-amber-500 to-yellow-400 p-2 rounded-xl shadow-lg transform -rotate-2">
                    <i class="fa-solid fa-flag-checkered text-xl text-slate-950"></i>
                </div>
                <div>
                    <h1 class="text-xl sm:text-2xl font-black italic tracking-wider bg-clip-text text-transparent bg-gradient-to-r from-red-500 via-amber-400 to-yellow-300">
                        SUPER POYGA
                    </h1>
                    <p id="userGreeting" class="text-xs text-slate-400 font-medium">+998 -- --- -- --</p>
                </div>
            </div>

            <!-- Header Controls -->
            <div class="flex items-center space-x-2 sm:space-x-3">
                <button id="soundToggleBtn" onclick="toggleSound()" class="p-2 sm:px-3 sm:py-2 rounded-xl bg-slate-800/80 hover:bg-slate-700 text-amber-400 border border-slate-700 transition-all text-xs sm:text-sm font-bold flex items-center gap-2">
                    <i id="soundIcon" class="fa-solid fa-volume-high"></i>
                    <span class="hidden sm:inline">Ovoz</span>
                </button>
                <button onclick="resetSetup()" class="px-3 py-2 rounded-xl bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-500 text-white font-bold text-xs uppercase tracking-wider shadow-md transition-all flex items-center space-x-2">
                    <i class="fa-solid fa-sliders"></i>
                    <span class="hidden sm:inline" id="txtBtnMenu">Sozlamalar</span>
                </button>
                <button onclick="logout()" class="p-2 sm:p-2.5 rounded-xl bg-red-500/20 hover:bg-red-500/30 text-red-400 border border-red-500/30 transition-all" title="Chiqish">
                    <i class="fa-solid fa-right-from-bracket text-sm"></i>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="flex-1 max-w-5xl w-full mx-auto p-4 flex flex-col justify-center my-auto">

        <!-- 1. LOGIN / SMS VERIFICATION SCREEN -->
        <section id="loginScreen" class="py-6 flex flex-col items-center justify-center">
            <div class="glass-panel max-w-md w-full p-6 sm:p-8 rounded-3xl border border-slate-700/80 shadow-2xl relative overflow-hidden">
                <div class="absolute -top-16 -right-16 w-36 h-36 bg-amber-500/20 rounded-full blur-2xl"></div>
                <div class="absolute -bottom-16 -left-16 w-36 h-36 bg-red-500/20 rounded-full blur-2xl"></div>

                <div class="text-center mb-6 relative z-10">
                    <div class="w-20 h-20 mx-auto mb-3 bg-gradient-to-tr from-red-600 via-amber-500 to-yellow-400 rounded-2xl flex items-center justify-center shadow-xl transform -rotate-3 border-2 border-amber-300">
                        <i class="fa-solid fa-car-side text-4xl text-slate-950"></i>
                    </div>
                    <h2 class="text-3xl font-black italic tracking-wide bg-clip-text text-transparent bg-gradient-to-r from-red-500 via-amber-400 to-yellow-300">
                        SUPER POYGA
                    </h2>
                    <p class="text-slate-400 text-xs mt-1">Kirish uchun telefon raqamingizni kiriting</p>
                </div>

                <!-- STEP 1: Phone Input -->
                <div id="phoneStep" class="space-y-4 relative z-10">
                    <div>
                        <label class="block text-xs font-bold text-slate-300 uppercase tracking-wider mb-1.5">Telefon raqamingiz</label>
                        <div class="relative">
                            <i class="fa-solid fa-phone absolute left-3.5 top-1/2 transform -translate-y-1/2 text-slate-500"></i>
                            <input type="tel" id="phoneNumber" value="+998 90 123 45 67" placeholder="+998 90 123 45 67" class="w-full bg-slate-900/90 border border-slate-700 rounded-xl py-3 pl-10 pr-4 text-sm font-semibold text-amber-300 focus:outline-none focus:border-amber-400 transition-all">
                        </div>
                    </div>

                    <button onclick="sendSMS()" class="w-full py-3.5 rounded-xl bg-gradient-to-r from-amber-500 to-yellow-500 hover:from-amber-400 hover:to-yellow-400 text-slate-950 font-black text-sm uppercase tracking-wider shadow-lg shadow-amber-500/20 transition-all transform hover:-translate-y-0.5 active:translate-y-0 flex items-center justify-center space-x-2">
                        <i class="fa-solid fa-paper-plane"></i>
                        <span>SMS Kod Yuborish</span>
                    </button>
                </div>

                <!-- STEP 2: Verification OTP Code Input -->
                <div id="otpStep" class="hidden space-y-4 relative z-10">
                    <div class="bg-amber-500/10 border border-amber-500/20 rounded-xl p-3 text-center">
                        <p class="text-xs text-amber-300">SMS kod telefoningizga yuborildi!</p>
                    </div>

                    <div>
                        <label class="block text-xs font-bold text-slate-300 uppercase tracking-wider mb-1.5">SMS KODNI KIRITING</label>
                        <div class="relative">
                            <i class="fa-solid fa-key absolute left-3.5 top-1/2 transform -translate-y-1/2 text-slate-500"></i>
                            <input type="text" id="otpInput" maxlength="6" placeholder="----" class="w-full bg-slate-900/90 border border-slate-700 rounded-xl py-3 pl-10 pr-4 text-xl font-bold tracking-widest text-center text-yellow-400 focus:outline-none focus:border-amber-400 transition-all">
                        </div>
                    </div>

                    <div id="loginError" class="hidden text-xs text-red-400 bg-red-500/10 p-2.5 rounded-xl border border-red-500/20 text-center font-semibold">
                        Kod xato kiratildi! SMS kodingizni tekshirib qayta kiriting.
                    </div>

                    <button onclick="verifyOTP()" class="w-full py-3.5 rounded-xl bg-gradient-to-r from-green-500 to-emerald-600 hover:from-green-400 hover:to-emerald-500 text-white font-black text-sm uppercase tracking-wider shadow-lg shadow-green-500/20 transition-all transform hover:-translate-y-0.5 active:translate-y-0 flex items-center justify-center space-x-2">
                        <i class="fa-solid fa-right-to-bracket"></i>
                        <span>KIRISH VA BOSHLASH</span>
                    </button>

                    <button onclick="resendSMS()" class="w-full text-center text-xs text-slate-400 hover:text-amber-400 transition-colors pt-2">
                        <i class="fa-solid fa-rotate-right mr-1"></i> Kod kelmadimi? Qayta yuborish
                    </button>
                </div>

            </div>
        </section>


        <!-- 2. MULTI-STEP SETUP WIZARD -->
        <section id="setupWizardScreen" class="hidden py-4">
            <div class="glass-panel max-w-2xl mx-auto p-6 sm:p-8 rounded-3xl border border-slate-700 shadow-2xl">
                
                <!-- Progress Stepper -->
                <div class="flex items-center justify-between mb-8 relative">
                    <div class="absolute left-0 top-1/2 transform -translate-y-1/2 w-full h-1 bg-slate-800 -z-0"></div>
                    <div id="stepLine" class="absolute left-0 top-1/2 transform -translate-y-1/2 h-1 bg-amber-500 transition-all duration-300 w-0 -z-0"></div>

                    <!-- Step 1 Dot -->
                    <div id="dotStep1" class="w-10 h-10 rounded-full bg-amber-500 text-slate-950 font-black flex items-center justify-center z-10 shadow-lg border-4 border-slate-900 transition-all">1</div>
                    <!-- Step 2 Dot -->
                    <div id="dotStep2" class="w-10 h-10 rounded-full bg-slate-800 text-slate-400 font-bold flex items-center justify-center z-10 shadow-lg border-4 border-slate-900 transition-all">2</div>
                    <!-- Step 3 Dot -->
                    <div id="dotStep3" class="w-10 h-10 rounded-full bg-slate-800 text-slate-400 font-bold flex items-center justify-center z-10 shadow-lg border-4 border-slate-900 transition-all">3</div>
                </div>

                <!-- STEP 1: SINF (Grade) -->
                <div id="wizardStep1" class="space-y-6">
                    <div class="text-center">
                        <h3 class="text-2xl font-black text-white italic tracking-wide" id="txtSelectGradeTitle">1. SINF NINI TANLANG</h3>
                        <p class="text-slate-400 text-xs mt-1" id="txtSelectGradeSub">1-sinfdan 11-sinfgacha yoki aralash darajani tanlang</p>
                    </div>

                    <div class="grid grid-cols-3 sm:grid-cols-4 gap-3">
                        <!-- 1-11 Grades -->
                        <script>
                            for(let i=1; i<=11; i++) {
                                document.write(`
                                    <button onclick="selectGrade(${i})" class="grade-btn glass-card p-3 rounded-2xl hover:bg-amber-500/20 border border-slate-700 hover:border-amber-400 text-center transition-all group">
                                        <div class="text-2xl font-black text-amber-400 group-hover:scale-110 transition-transform">${i}</div>
                                        <div class="text-[10px] text-slate-400 uppercase font-bold mt-1">${i}-Sinf</div>
                                    </button>
                                `);
                            }
                        </script>
                        <!-- Mixed Grade -->
                        <button onclick="selectGrade('all')" class="grade-btn glass-card p-3 rounded-2xl bg-gradient-to-br from-indigo-900/40 to-purple-900/40 border border-indigo-500/40 hover:border-indigo-400 text-center transition-all group col-span-3 sm:col-span-1">
                            <div class="text-xl font-black text-indigo-300 group-hover:scale-110 transition-transform"><i class="fa-solid fa-shuffle"></i></div>
                            <div class="text-[10px] text-indigo-300 font-bold mt-1">Aralash</div>
                        </button>
                    </div>
                </div>

                <!-- STEP 2: FAN (Subject) -->
                <div id="wizardStep2" class="hidden space-y-6">
                    <div class="text-center">
                        <h3 class="text-2xl font-black text-white italic tracking-wide" id="txtSelectSubjectTitle">2. FANNI TANLANG</h3>
                        <p class="text-slate-400 text-xs mt-1" id="txtSelectSubjectSub">O'zingiz kuchli bo'lgan fanni tanlang</p>
                    </div>

                    <div id="subjectContainer" class="grid grid-cols-2 sm:grid-cols-3 gap-3">
                        <!-- Subjects will be dynamically inserted -->
                    </div>

                    <button onclick="goToWizardStep(1)" class="w-full py-2.5 rounded-xl bg-slate-800 text-slate-300 text-xs font-bold uppercase tracking-wider hover:bg-slate-700 transition-all">
                        <i class="fa-solid fa-arrow-left mr-2"></i> Orqaga (Sinf)
                    </button>
                </div>

                <!-- STEP 3: TIL (Language) -->
                <div id="wizardStep3" class="hidden space-y-6">
                    <div class="text-center">
                        <h3 class="text-2xl font-black text-white italic tracking-wide" id="txtSelectLangTitle">3. MULOQOT TILINI TANLANG</h3>
                        <p class="text-slate-400 text-xs mt-1" id="txtSelectLangSub">Savollar va interfeys ushbu tilda bo'ladi</p>
                    </div>

                    <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                        <button onclick="selectLanguage('uz')" class="glass-card p-5 rounded-2xl hover:bg-amber-500/20 border border-slate-700 hover:border-amber-400 text-center transition-all flex flex-col items-center justify-center space-y-2 group">
                            <span class="text-3xl">🇺🇿</span>
                            <span class="font-bold text-white group-hover:text-amber-400">O'zbekcha</span>
                        </button>
                        <button onclick="selectLanguage('ru')" class="glass-card p-5 rounded-2xl hover:bg-amber-500/20 border border-slate-700 hover:border-amber-400 text-center transition-all flex flex-col items-center justify-center space-y-2 group">
                            <span class="text-3xl">🇷🇺</span>
                            <span class="font-bold text-white group-hover:text-amber-400">Русский</span>
                        </button>
                        <button onclick="selectLanguage('en')" class="glass-card p-5 rounded-2xl hover:bg-amber-500/20 border border-slate-700 hover:border-amber-400 text-center transition-all flex flex-col items-center justify-center space-y-2 group">
                            <span class="text-3xl">🇬🇧</span>
                            <span class="font-bold text-white group-hover:text-amber-400">English</span>
                        </button>
                    </div>

                    <button onclick="goToWizardStep(2)" class="w-full py-2.5 rounded-xl bg-slate-800 text-slate-300 text-xs font-bold uppercase tracking-wider hover:bg-slate-700 transition-all">
                        <i class="fa-solid fa-arrow-left mr-2"></i> Orqaga (Fanlar)
                    </button>
                </div>

            </div>
        </section>


        <!-- 3. MAIN GAME ARENA & RACE TRACK -->
        <section id="gameScreen" class="hidden space-y-4">

            <!-- Top Dashboard / Stats -->
            <div class="grid grid-cols-2 sm:grid-cols-4 gap-3">
                <div class="glass-card p-3 rounded-2xl border border-slate-700 flex items-center space-x-3">
                    <div class="p-2.5 rounded-xl bg-amber-500/20 text-amber-400">
                        <i class="fa-solid fa-graduation-cap text-lg"></i>
                    </div>
                    <div>
                        <div class="text-[10px] text-slate-400 uppercase font-bold" id="lblGrade">Sinf</div>
                        <div id="gameGradeDisplay" class="text-sm font-black text-white">---</div>
                    </div>
                </div>

                <div class="glass-card p-3 rounded-2xl border border-slate-700 flex items-center space-x-3">
                    <div class="p-2.5 rounded-xl bg-blue-500/20 text-blue-400">
                        <i class="fa-solid fa-book text-lg"></i>
                    </div>
                    <div>
                        <div class="text-[10px] text-slate-400 uppercase font-bold" id="lblSubject">Fan</div>
                        <div id="gameSubjectDisplay" class="text-sm font-black text-white truncate max-w-[100px]">---</div>
                    </div>
                </div>

                <div class="glass-card p-3 rounded-2xl border border-slate-700 flex items-center space-x-3">
                    <div class="p-2.5 rounded-xl bg-green-500/20 text-green-400">
                        <i class="fa-solid fa-gauge-high text-lg"></i>
                    </div>
                    <div>
                        <div class="text-[10px] text-slate-400 uppercase font-bold" id="lblSpeed">Tezlik</div>
                        <div id="speedometerDisplay" class="text-sm font-black text-green-400">0 KM/H</div>
                    </div>
                </div>

                <div class="glass-card p-3 rounded-2xl border border-slate-700 flex items-center space-x-3">
                    <div class="p-2.5 rounded-xl bg-red-500/20 text-red-400">
                        <i class="fa-solid fa-trophy text-lg"></i>
                    </div>
                    <div>
                        <div class="text-[10px] text-slate-400 uppercase font-bold" id="lblScore">Ochko</div>
                        <div class="text-sm font-black text-amber-400">
                            <span id="scoreDisplay">0</span> / 10
                        </div>
                    </div>
                </div>
            </div>

            <!-- 2D Race Track -->
            <div class="road-container p-4 shadow-2xl relative">
                <div class="flex justify-between items-center mb-2 px-2">
                    <span class="text-xs font-black text-amber-400 uppercase tracking-widest flex items-center gap-1.5">
                        <i class="fa-solid fa-flag-checkered"></i> POYGA TRASSASI
                    </span>
                    <span class="text-xs font-bold text-slate-400" id="txtRaceGoal">Marra: 10 Ball</span>
                </div>

                <div class="road-lane flex items-center">
                    <div class="road-finish-line"></div>
                    
                    <!-- Player Car Sprite -->
                    <div id="playerCar" class="car-sprite" style="left: 10px;">
                        <div class="exhaust-flame"></div>
                        <div class="relative transform hover:scale-105 transition-transform">
                            <div class="w-14 h-8 bg-gradient-to-r from-red-600 via-red-500 to-amber-500 rounded-lg shadow-lg border border-amber-300 flex items-center justify-center relative">
                                <i class="fa-solid fa-car-side text-white text-xl"></i>
                                <!-- Spoiler -->
                                <div class="absolute -left-1.5 top-1 bottom-1 w-2 bg-slate-900 rounded-l"></div>
                                <!-- Wheels -->
                                <div class="absolute -bottom-2 left-2 w-3.5 h-3.5 bg-slate-950 rounded-full border-2 border-slate-400"></div>
                                <div class="absolute -bottom-2 right-2 w-3.5 h-3.5 bg-slate-950 rounded-full border-2 border-slate-400"></div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Question Card -->
            <div class="glass-panel p-6 rounded-3xl border border-slate-700 shadow-2xl space-y-6">
                
                <div class="flex items-center justify-between border-b border-slate-800 pb-3">
                    <span class="text-xs font-bold text-amber-400 uppercase tracking-wider flex items-center gap-2">
                        <i class="fa-solid fa-circle-question"></i>
                        <span id="txtQuestionNum">Savol 1 / 10</span>
                    </span>
                    <span id="subjectBadge" class="px-3 py-1 rounded-full bg-slate-800 text-slate-300 text-[11px] font-bold border border-slate-700">
                        Matematika
                    </span>
                </div>

                <!-- Question Text -->
                <div class="text-center py-2">
                    <h2 id="questionText" class="text-xl sm:text-2xl font-black text-white leading-relaxed">
                        Savol yuklanmoqda...
                    </h2>
                </div>

                <!-- Options Grid -->
                <div id="optionsContainer" class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                    <!-- Options injected dynamically -->
                </div>

            </div>
        </section>


        <!-- 4. VICTORY MODAL SCREEN -->
        <section id="victoryScreen" class="hidden py-6">
            <div class="glass-panel max-w-lg mx-auto p-8 rounded-3xl border-2 border-amber-400/50 shadow-2xl text-center space-y-6 relative overflow-hidden nitro-active">
                
                <div class="w-28 h-28 mx-auto bg-gradient-to-tr from-yellow-400 via-amber-500 to-red-500 rounded-full flex items-center justify-center shadow-2xl border-4 border-yellow-300 transform hover:rotate-12 transition-transform">
                    <i class="fa-solid fa-trophy text-6xl text-slate-950"></i>
                </div>

                <div>
                    <h2 class="text-3xl font-black italic tracking-wider bg-clip-text text-transparent bg-gradient-to-r from-yellow-300 via-amber-400 to-red-500" id="txtVictoryTitle">
                        G'ALABA! SIZ MARRAGA YETDINGIZ!
                    </h2>
                    <p class="text-slate-300 text-sm mt-2" id="txtVictorySub">
                        Barcha 10 ta savolga to'g'ri javob berib, poygada chempion bo'ldingiz!
                    </p>
                </div>

                <div class="glass-card p-4 rounded-2xl border border-slate-700 max-w-xs mx-auto">
                    <div class="text-xs text-slate-400 uppercase font-bold" id="lblFinalScore">Yakuniy natija</div>
                    <div class="text-3xl font-black text-amber-400 mt-1">10 / 10 BALL</div>
                </div>

                <div class="flex flex-col sm:flex-row gap-3">
                    <button onclick="restartGame()" class="flex-1 py-3.5 rounded-xl bg-gradient-to-r from-amber-500 to-yellow-500 hover:from-amber-400 hover:to-yellow-400 text-slate-950 font-black text-sm uppercase tracking-wider shadow-lg transition-all flex items-center justify-center space-x-2">
                        <i class="fa-solid fa-rotate-right"></i>
                        <span id="btnPlayAgain">Qayta O'ynash</span>
                    </button>
                    <button onclick="resetSetup()" class="flex-1 py-3.5 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-200 font-bold text-sm uppercase tracking-wider transition-all flex items-center justify-center space-x-2">
                        <i class="fa-solid fa-sliders"></i>
                        <span id="btnNewCategory">Yangi Sinf/Fan</span>
                    </button>
                </div>

            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="text-center py-4 text-xs text-slate-500 border-t border-slate-900/60">
        Super Poyga &copy; 2026 - O'quvchilar uchun bilim va tezlik musobaqasi
    </footer>

    <!-- JS LOGIC -->
    <script>
        /* ==================== STATE MANAGEMENT ==================== */
        let state = {
            phoneNumber: '',
            generatedOTP: '',
            isLoggedIn: false,
            grade: null, // 1-11 or 'all'
            subject: null, // 'math', 'lang', 'physics', 'history', etc.
            language: 'uz', // 'uz', 'ru', 'en'
            score: 0,
            currentQuestionIndex: 0,
            soundEnabled: true,
            questions: []
        };

        /* Web Audio API Sound Synthesizer */
        const AudioContext = window.AudioContext || window.webkitAudioContext;
        let audioCtx = null;

        function initAudio() {
            if(!audioCtx) {
                audioCtx = new AudioContext();
            }
        }

        function playSound(type) {
            if(!state.soundEnabled) return;
            try {
                initAudio();
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.connect(gain);
                gain.connect(audioCtx.destination);

                if(type === 'correct') {
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(523.25, audioCtx.currentTime); // C5
                    osc.frequency.exponentialRampToValueAtTime(880, audioCtx.currentTime + 0.3); // A5
                    gain.gain.setValueAtTime(0.3, audioCtx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.3);
                    osc.start();
                    osc.stop(audioCtx.currentTime + 0.3);
                } else if(type === 'wrong') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(180, audioCtx.currentTime);
                    osc.frequency.exponentialRampToValueAtTime(110, audioCtx.currentTime + 0.3);
                    gain.gain.setValueAtTime(0.3, audioCtx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.3);
                    osc.start();
                    osc.stop(audioCtx.currentTime + 0.3);
                } else if(type === 'win') {
                    const now = audioCtx.currentTime;
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(440, now);
                    osc.frequency.setValueAtTime(554.37, now + 0.15);
                    osc.frequency.setValueAtTime(659.25, now + 0.3);
                    osc.frequency.setValueAtTime(880, now + 0.45);
                    gain.gain.setValueAtTime(0.4, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.8);
                    osc.start();
                    osc.stop(now + 0.8);
                }
            } catch(e) {
                console.log("Audio not supported or restricted");
            }
        }

        function toggleSound() {
            state.soundEnabled = !state.soundEnabled;
            const icon = document.getElementById('soundIcon');
            if(state.soundEnabled) {
                icon.className = 'fa-solid fa-volume-high';
            } else {
                icon.className = 'fa-solid fa-volume-xmark text-slate-500';
            }
        }


        /* ==================== SMS / LOGIN SYSTEM ==================== */
        function sendSMS() {
            const phone = document.getElementById('phoneNumber').value.trim();
            if(!phone) return;

            state.phoneNumber = phone;
            // Generate Random 4-digit code
            state.generatedOTP = Math.floor(1000 + Math.random() * 9000).toString();

            // Show Toast SMS
            document.getElementById('smsCodeDisplay').innerText = state.generatedOTP;
            const toast = document.getElementById('smsToast');
            toast.classList.remove('hidden');

            // Switch to OTP Input
            document.getElementById('phoneStep').classList.add('hidden');
            document.getElementById('otpStep').classList.remove('hidden');
        }

        function resendSMS() {
            sendSMS();
        }

        function hideSmsToast() {
            document.getElementById('smsToast').classList.add('hidden');
        }

        function copyCode() {
            const code = state.generatedOTP;
            document.getElementById('otpInput').value = code;
            hideSmsToast();
        }

        function verifyOTP() {
            const userOTP = document.getElementById('otpInput').value.trim();
            if(userOTP === state.generatedOTP) {
                state.isLoggedIn = true;
                hideSmsToast();
                document.getElementById('loginScreen').classList.add('hidden');
                document.getElementById('appHeader').classList.remove('hidden');
                document.getElementById('userGreeting').innerText = state.phoneNumber;
                
                // Show Setup Wizard
                document.getElementById('setupWizardScreen').classList.remove('hidden');
                goToWizardStep(1);
            } else {
                document.getElementById('loginError').classList.remove('hidden');
            }
        }

        function logout() {
            state.isLoggedIn = false;
            document.getElementById('appHeader').classList.add('hidden');
            document.getElementById('setupWizardScreen').classList.add('hidden');
            document.getElementById('gameScreen').classList.add('hidden');
            document.getElementById('victoryScreen').classList.add('hidden');
            document.getElementById('loginScreen').classList.remove('hidden');
            
            document.getElementById('phoneStep').classList.remove('hidden');
            document.getElementById('otpStep').classList.add('hidden');
            document.getElementById('otpInput').value = '';
            document.getElementById('loginError').classList.add('hidden');
        }


        /* ==================== MULTI-STEP SETUP WIZARD ==================== */
        const subjectsList = [
            { id: 'math', name: { uz: 'Matematika', ru: 'Математика', en: 'Mathematics' }, icon: 'fa-calculator', color: 'from-blue-600 to-indigo-600' },
            { id: 'lang', name: { uz: "O'zbek tili", ru: 'Узбекский язык', en: 'Uzbek Language' }, icon: 'fa-language', color: 'from-amber-600 to-yellow-600' },
            { id: 'physics', name: { uz: 'Fizika', ru: 'Физика', en: 'Physics' }, icon: 'fa-atom', color: 'from-purple-600 to-pink-600' },
            { id: 'chemistry', name: { uz: 'Kimyo', ru: 'Химия', en: 'Chemistry' }, icon: 'fa-flask', color: 'from-emerald-600 to-teal-600' },
            { id: 'biology', name: { uz: 'Biologiya', ru: 'Биология', en: 'Biology' }, icon: 'fa-dna', color: 'from-green-600 to-emerald-600' },
            { id: 'history', name: { uz: 'Tarix', ru: 'История', en: 'History' }, icon: 'fa-landmark', color: 'from-red-600 to-amber-600' },
            { id: 'mantiq', name: { uz: 'Mantiq', ru: 'Логика', en: 'Logic' }, icon: 'fa-brain', color: 'from-cyan-600 to-blue-600' }
        ];

        function goToWizardStep(step) {
            document.getElementById('wizardStep1').classList.add('hidden');
            document.getElementById('wizardStep2').classList.add('hidden');
            document.getElementById('wizardStep3').classList.add('hidden');

            const line = document.getElementById('stepLine');
            const d1 = document.getElementById('dotStep1');
            const d2 = document.getElementById('dotStep2');
            const d3 = document.getElementById('dotStep3');

            if(step === 1) {
                document.getElementById('wizardStep1').classList.remove('hidden');
                line.style.width = '0%';
                d1.className = 'w-10 h-10 rounded-full bg-amber-500 text-slate-950 font-black flex items-center justify-center z-10 shadow-lg border-4 border-slate-900';
                d2.className = 'w-10 h-10 rounded-full bg-slate-800 text-slate-400 font-bold flex items-center justify-center z-10 shadow-lg border-4 border-slate-900';
                d3.className = 'w-10 h-10 rounded-full bg-slate-800 text-slate-400 font-bold flex items-center justify-center z-10 shadow-lg border-4 border-slate-900';
            } else if(step === 2) {
                document.getElementById('wizardStep2').classList.remove('hidden');
                line.style.width = '50%';
                d2.className = 'w-10 h-10 rounded-full bg-amber-500 text-slate-950 font-black flex items-center justify-center z-10 shadow-lg border-4 border-slate-900';
                d3.className = 'w-10 h-10 rounded-full bg-slate-800 text-slate-400 font-bold flex items-center justify-center z-10 shadow-lg border-4 border-slate-900';
                renderSubjects();
            } else if(step === 3) {
                document.getElementById('wizardStep3').classList.remove('hidden');
                line.style.width = '100%';
                d3.className = 'w-10 h-10 rounded-full bg-amber-500 text-slate-950 font-black flex items-center justify-center z-10 shadow-lg border-4 border-slate-900';
            }
        }

        function selectGrade(g) {
            state.grade = g;
            goToWizardStep(2);
        }

        function renderSubjects() {
            const container = document.getElementById('subjectContainer');
            container.innerHTML = '';

            subjectsList.forEach(s => {
                const name = s.name[state.language] || s.name['uz'];
                container.innerHTML += `
                    <button onclick="selectSubject('${s.id}')" class="glass-card p-4 rounded-2xl hover:bg-amber-500/20 border border-slate-700 hover:border-amber-400 text-left transition-all flex items-center space-x-3 group">
                        <div class="p-3 rounded-xl bg-gradient-to-tr ${s.color} text-white group-hover:scale-110 transition-transform shadow-md">
                            <i class="fa-solid ${s.icon} text-lg"></i>
                        </div>
                        <span class="font-bold text-sm text-white group-hover:text-amber-400">${name}</span>
                    </button>
                `;
            });
        }

        function selectSubject(sub) {
            state.subject = sub;
            goToWizardStep(3);
        }

        function selectLanguage(lang) {
            state.language = lang;
            updateUILabels();
            startGame();
        }

        function resetSetup() {
            document.getElementById('gameScreen').classList.add('hidden');
            document.getElementById('victoryScreen').classList.add('hidden');
            document.getElementById('setupWizardScreen').classList.remove('hidden');
            goToWizardStep(1);
        }


        /* ==================== MULTI-LANGUAGE UI LABELS ==================== */
        const i18n = {
            uz: {
                txtBtnMenu: "Sozlamalar",
                txtSelectGradeTitle: "1. SINF NINI TANLANG",
                txtSelectGradeSub: "1-sinfdan 11-sinfgacha yoki aralash darajani tanlang",
                txtSelectSubjectTitle: "2. FANNI TANLANG",
                txtSelectSubjectSub: "O'zingiz kuchli bo'lgan fanni tanlang",
                txtSelectLangTitle: "3. MULOQOT TILINI TANLANG",
                txtSelectLangSub: "Savollar va interfeys ushbu tilda bo'ladi",
                lblGrade: "Sinf",
                lblSubject: "Fan",
                lblSpeed: "Tezlik",
                lblScore: "Ochko",
                txtRaceGoal: "Marra: 10 Ball",
                txtVictoryTitle: "G'ALABA! SIZ MARRAGA YETDINGIZ!",
                txtVictorySub: "Barcha 10 ta savolga to'g'ri javob berib, poygada chempion bo'ldingiz!",
                lblFinalScore: "Yakuniy natija",
                btnPlayAgain: "Qayta O'ynash",
                btnNewCategory: "Yangi Sinf/Fan"
            },
            ru: {
                txtBtnMenu: "Настройки",
                txtSelectGradeTitle: "1. ВЫБЕРИТЕ КЛАСС",
                txtSelectGradeSub: "С 1 по 11 класс или смешанный уровень",
                txtSelectSubjectTitle: "2. ВЫБЕРИТЕ ПРЕДМЕТ",
                txtSelectSubjectSub: "Выберите ваш любимый предмет",
                txtSelectLangTitle: "3. ВЫБЕРИТЕ ЯЗЫК",
                txtSelectLangSub: "Вопросы и интерфейс будут на этом языке",
                lblGrade: "Класс",
                lblSubject: "Предмет",
                lblSpeed: "Скорость",
                lblScore: "Очки",
                txtRaceGoal: "Финиш: 10 Очков",
                txtVictoryTitle: "ПОБЕДА! ВЫ ДОСТИГЛИ ФИНИША!",
                txtVictorySub: "Вы правильно ответили на 10 вопросов и стали чемпионом гонки!",
                lblFinalScore: "Итоговый результат",
                btnPlayAgain: "Играть снова",
                btnNewCategory: "Новый класс/предмет"
            },
            en: {
                txtBtnMenu: "Settings",
                txtSelectGradeTitle: "1. SELECT GRADE",
                txtSelectGradeSub: "From Grade 1 to 11 or mixed difficulty",
                txtSelectSubjectTitle: "2. SELECT SUBJECT",
                txtSelectSubjectSub: "Choose your strongest subject",
                txtSelectLangTitle: "3. SELECT LANGUAGE",
                txtSelectLangSub: "Questions and UI will be presented in this language",
                lblGrade: "Grade",
                lblSubject: "Subject",
                lblSpeed: "Speed",
                lblScore: "Score",
                txtRaceGoal: "Finish: 10 Pts",
                txtVictoryTitle: "VICTORY! YOU REACHED THE FINISH!",
                txtVictorySub: "You answered 10 questions correctly and won the race!",
                lblFinalScore: "Final Score",
                btnPlayAgain: "Play Again",
                btnNewCategory: "New Grade/Subject"
            }
        };

        function updateUILabels() {
            const lang = state.language || 'uz';
            const dict = i18n[lang];

            for (const key in dict) {
                const el = document.getElementById(key);
                if(el) el.innerText = dict[key];
            }
        }


        /* ==================== DYNAMIC QUESTION GENERATOR ==================== */
        function generateQuestions() {
            const list = [];
            const sub = state.subject || 'math';
            const grade = state.grade || 1;
            const lang = state.language || 'uz';

            for(let i = 1; i <= 10; i++) {
                let qText = '';
                let opts = [];
                let correctIdx = 0;

                if(sub === 'math') {
                    if(grade <= 4) {
                        const a = Math.floor(Math.random() * 20) + 1;
                        const b = Math.floor(Math.random() * 20) + 1;
                        const ans = a + b;
                        qText = `${a} + ${b} = ?`;
                        opts = [ans, ans + 2, Math.max(1, ans - 3), ans + 5];
                    } else if(grade <= 8) {
                        const a = Math.floor(Math.random() * 12) + 2;
                        const b = Math.floor(Math.random() * 12) + 2;
                        const ans = a * b;
                        qText = `${a} × ${b} = ?`;
                        opts = [ans, ans + 4, Math.max(2, ans - 5), ans + 10];
                    } else {
                        const x = Math.floor(Math.random() * 10) + 1;
                        const ans = x * x;
                        qText = `x² = ${ans} (x = ?)`;
                        opts = [x, x + 1, x + 2, Math.max(1, x - 1)];
                    }
                } else if(sub === 'mantiq') {
                    if(lang === 'uz') {
                        qText = `Mantiqiy savol #${i}: Keyingi sonni toping: ${i*2}, ${i*2+2}, ${i*2+4}, ?`;
                        const ans = i*2+6;
                        opts = [ans, ans+1, ans-2, ans+3];
                    } else if(lang === 'ru') {
                        qText = `Логика #${i}: Найдите следующее число: ${i*2}, ${i*2+2}, ${i*2+4}, ?`;
                        const ans = i*2+6;
                        opts = [ans, ans+1, ans-2, ans+3];
                    } else {
                        qText = `Logic #${i}: Find the next number: ${i*2}, ${i*2+2}, ${i*2+4}, ?`;
                        const ans = i*2+6;
                        opts = [ans, ans+1, ans-2, ans+3];
                    }
                } else {
                    // Generic Subject Questions
                    if(lang === 'uz') {
                        qText = `${state.subject.toUpperCase()} (${grade}-sinf) bo'yicha #${i} savol: To'g'ri variantni tanlang.`;
                        opts = ['To\'g\'ri javob', 'Noto\'g\'ri variant A', 'Noto\'g\'ri variant B', 'Noto\'g\'ri variant C'];
                    } else if(lang === 'ru') {
                        qText = `Вопрос #${i} по предмету ${state.subject.toUpperCase()} (${grade} класс): Выберите верный ответ.`;
                        opts = ['Правильный ответ', 'Неверный вариант А', 'Неверный вариант Б', 'Неверный вариант В'];
                    } else {
                        qText = `Question #${i} for ${state.subject.toUpperCase()} (Grade ${grade}): Select correct answer.`;
                        opts = ['Correct Answer', 'Wrong Option A', 'Wrong Option B', 'Wrong Option C'];
                    }
                }

                // Shuffle options
                const shuffledOpts = [...opts].sort(() => Math.random() - 0.5);
                correctIdx = shuffledOpts.indexOf(opts[0]);

                list.push({
                    text: qText,
                    options: shuffledOpts.map(o => o.toString()),
                    correct: correctIdx
                });
            }

            return list;
        }


        /* ==================== GAMEPLAY LOGIC ==================== */
        function startGame() {
            state.score = 0;
            state.currentQuestionIndex = 0;
            state.questions = generateQuestions();

            document.getElementById('setupWizardScreen').classList.add('hidden');
            document.getElementById('victoryScreen').classList.add('hidden');
            document.getElementById('gameScreen').classList.remove('hidden');

            // Dashboard info
            document.getElementById('gameGradeDisplay').innerText = state.grade === 'all' ? 'Aralash' : `${state.grade}-Sinf`;
            const subObj = subjectsList.find(s => s.id === state.subject);
            document.getElementById('gameSubjectDisplay').innerText = subObj ? (subObj.name[state.language] || subObj.name['uz']) : state.subject;
            document.getElementById('subjectBadge').innerText = subObj ? (subObj.name[state.language] || subObj.name['uz']) : state.subject;

            updateScoreAndCar();
            renderCurrentQuestion();
        }

        function renderCurrentQuestion() {
            if(state.currentQuestionIndex >= state.questions.length) {
                finishGame();
                return;
            }

            const q = state.questions[state.currentQuestionIndex];
            document.getElementById('txtQuestionNum').innerText = `Savol ${state.currentQuestionIndex + 1} / 10`;
            document.getElementById('questionText').innerText = q.text;

            const optsContainer = document.getElementById('optionsContainer');
            optsContainer.innerHTML = '';

            q.options.forEach((optText, idx) => {
                optsContainer.innerHTML += `
                    <button onclick="submitAnswer(${idx})" class="p-4 rounded-2xl glass-card hover:bg-amber-500/20 border border-slate-700 hover:border-amber-400 text-left font-bold text-white transition-all transform hover:-translate-y-0.5 active:translate-y-0 flex items-center space-x-3 group">
                        <span class="w-8 h-8 rounded-xl bg-slate-800 text-amber-400 border border-slate-700 flex items-center justify-center font-black text-sm group-hover:bg-amber-500 group-hover:text-slate-950 transition-colors">
                            ${String.fromCharCode(65 + idx)}
                        </span>
                        <span class="text-sm flex-1">${optText}</span>
                    </button>
                `;
            });
        }

        function submitAnswer(selectedIdx) {
            const q = state.questions[state.currentQuestionIndex];

            if(selectedIdx === q.correct) {
                // Correct
                state.score++;
                playSound('correct');
                triggerNitro();
            } else {
                // Wrong
                playSound('wrong');
            }

            state.currentQuestionIndex++;
            updateScoreAndCar();

            if(state.score >= 10) {
                setTimeout(finishGame, 600);
            } else {
                renderCurrentQuestion();
            }
        }

        function updateScoreAndCar() {
            document.getElementById('scoreDisplay').innerText = state.score;
            const speed = state.score * 22;
            document.getElementById('speedometerDisplay').innerText = `${speed} KM/H`;

            // Calculate car position (0% to 85% across track)
            const trackPercent = (state.score / 10) * 82;
            const car = document.getElementById('playerCar');
            car.style.left = `calc(${trackPercent}% + 10px)`;
        }

        function triggerNitro() {
            const car = document.getElementById('playerCar');
            car.classList.add('moving', 'nitro-active');
            setTimeout(() => {
                car.classList.remove('moving', 'nitro-active');
            }, 600);
        }

        function finishGame() {
            playSound('win');
            confetti({
                particleCount: 120,
                spread: 80,
                origin: { y: 0.6 }
            });

            document.getElementById('gameScreen').classList.add('hidden');
            document.getElementById('victoryScreen').classList.remove('hidden');
        }

        function restartGame() {
            startGame();
        }
    </script>
</body>
</html>
