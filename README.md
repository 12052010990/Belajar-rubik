<!DOCTYPE html>
<html lang="id" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RasyaCube - Master Your Rubik's Cube</title>
    
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        primary: '#3b82f6', // Blue 500
                        primaryHover: '#2563eb', // Blue 600
                        pastel: '#eff6ff', // Blue 50
                        pastelDark: '#dbeafe', // Blue 100
                        darkText: '#1e3a8a', // Blue 900
                    },
                    boxShadow: {
                        'soft': '0 10px 40px -10px rgba(59, 130, 246, 0.15)',
                    }
                }
            }
        }
    </script>

    <!-- FontAwesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <style>
        /* Custom Styles & Animations */
        body {
            background-color: #fafafa;
        }
        
        .glass-nav {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border-bottom: 1px solid rgba(255,255,255,0.3);
        }

        .floating-cube {
            animation: float 6s ease-in-out infinite;
        }

        @keyframes float {
            0% { transform: translateY(0px) rotate(0deg); }
            50% { transform: translateY(-20px) rotate(5deg); }
            100% { transform: translateY(0px) rotate(0deg); }
        }

        /* Toast Notification */
        #toast {
            visibility: hidden;
            opacity: 0;
            transition: visibility 0s, opacity 0.3s linear;
        }
        #toast.show {
            visibility: visible;
            opacity: 1;
        }

        /* Hide scrollbar for clean aesthetic */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #eff6ff; 
        }
        ::-webkit-scrollbar-thumb {
            background: #93c5fd; 
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #60a5fa; 
        }
    </style>
</head>
<body class="text-gray-700 antialiased font-sans selection:bg-primary selection:text-white">

    <!-- Progress Bar (Fixed Top) -->
    <div class="fixed top-0 left-0 w-full h-1 bg-gray-200 z-50">
        <div id="learning-progress" class="h-full bg-primary transition-all duration-500" style="width: 0%;"></div>
    </div>

    <!-- Toast Notification (Replaces alert) -->
    <div id="toast" class="fixed bottom-5 right-5 z-50 bg-green-500 text-white px-6 py-3 rounded-xl shadow-lg flex items-center gap-3">
        <i class="fa-solid fa-check-circle"></i>
        <span id="toast-msg">Rumus berhasil disalin!</span>
    </div>

    <!-- Navbar -->
    <nav class="glass-nav fixed w-full top-1 z-40 transition-all duration-300">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between items-center h-20">
                <!-- Logo -->
                <div class="flex-shrink-0 flex items-center gap-2 cursor-pointer" onclick="window.scrollTo(0,0)">
                    <div class="w-10 h-10 bg-primary text-white rounded-xl flex items-center justify-center shadow-soft">
                        <i class="fa-solid fa-cube text-xl"></i>
                    </div>
                    <span class="font-bold text-2xl tracking-tight text-darkText">Rasya<span class="text-primary">Cube</span></span>
                </div>
                
                <!-- Desktop Menu -->
                <div class="hidden md:flex items-center space-x-8">
                    <a href="#home" class="text-gray-600 hover:text-primary transition font-medium">Home</a>
                    <a href="#tutorial" class="text-gray-600 hover:text-primary transition font-medium">Tutorial</a>
                    <a href="#rumus" class="text-gray-600 hover:text-primary transition font-medium">Rumus</a>
                    <a href="#quiz" class="text-gray-600 hover:text-primary transition font-medium">Quiz</a>
                    <a href="#about" class="text-gray-600 hover:text-primary transition font-medium">About</a>
                    <a href="#tutorial" class="bg-primary hover:bg-primaryHover text-white px-6 py-2.5 rounded-full font-medium transition shadow-md hover:shadow-lg transform hover:-translate-y-0.5">
                        Mulai Belajar
                    </a>
                </div>

                <!-- Mobile Menu Button -->
                <div class="md:hidden flex items-center">
                    <button id="mobile-menu-btn" class="text-gray-600 hover:text-primary focus:outline-none text-2xl">
                        <i class="fa-solid fa-bars"></i>
                    </button>
                </div>
            </div>
        </div>

        <!-- Mobile Menu (Hidden by default) -->
        <div id="mobile-menu" class="hidden md:hidden bg-white border-t border-gray-100 absolute w-full shadow-lg">
            <div class="px-4 pt-2 pb-6 space-y-2">
                <a href="#home" class="block px-3 py-3 rounded-md text-base font-medium text-gray-700 hover:text-primary hover:bg-pastel transition">Home</a>
                <a href="#tutorial" class="block px-3 py-3 rounded-md text-base font-medium text-gray-700 hover:text-primary hover:bg-pastel transition">Tutorial</a>
                <a href="#rumus" class="block px-3 py-3 rounded-md text-base font-medium text-gray-700 hover:text-primary hover:bg-pastel transition">Rumus</a>
                <a href="#quiz" class="block px-3 py-3 rounded-md text-base font-medium text-gray-700 hover:text-primary hover:bg-pastel transition">Quiz</a>
                <a href="#about" class="block px-3 py-3 rounded-md text-base font-medium text-gray-700 hover:text-primary hover:bg-pastel transition">About</a>
            </div>
        </div>
    </nav>

    <!-- Hero Section -->
    <section id="home" class="pt-32 pb-20 lg:pt-48 lg:pb-32 bg-gradient-to-b from-pastel to-white min-h-screen flex items-center">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 w-full">
            <div class="flex flex-col-reverse lg:flex-row items-center justify-between gap-12">
                
                <!-- Text Content -->
                <div class="w-full lg:w-1/2 text-center lg:text-left">
                    <div class="inline-flex items-center gap-2 px-4 py-2 rounded-full bg-blue-100 text-primary font-medium text-sm mb-6">
                        <i class="fa-solid fa-rocket"></i> Platform Belajar Rubik No. 1
                    </div>
                    <h1 class="text-4xl md:text-5xl lg:text-6xl font-extrabold text-darkText leading-tight mb-6">
                        Master Your <br/>
                        <span class="text-transparent bg-clip-text bg-gradient-to-r from-primary to-blue-400">Rubik's Cube</span>
                    </h1>
                    <p class="text-lg md:text-xl text-gray-600 mb-8 max-w-2xl mx-auto lg:mx-0">
                        Belajar menyelesaikan Rubik dari nol hingga mahir dengan cara yang interaktif, terstruktur, dan menyenangkan. Siap untuk tantangan?
                    </p>
                    <div class="flex flex-col sm:flex-row items-center justify-center lg:justify-start gap-4">
                        <a href="#tutorial" class="w-full sm:w-auto text-center bg-primary hover:bg-primaryHover text-white px-8 py-3.5 rounded-full font-medium transition shadow-soft hover:shadow-lg transform hover:-translate-y-1 text-lg">
                            Mulai Belajar
                        </a>
                        <a href="#rumus" class="w-full sm:w-auto text-center bg-white border-2 border-primary text-primary hover:bg-pastel px-8 py-3.5 rounded-full font-medium transition text-lg">
                            Jelajahi Rumus
                        </a>
                    </div>
                </div>

                <!-- 3D Graphic (SVG) -->
                <div class="w-full lg:w-1/2 flex justify-center floating-cube">
                    <!-- Custom SVG Rubik's Cube Isometric Illustration -->
                    <svg width="400" height="400" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg" class="w-72 md:w-96 lg:w-full max-w-md drop-shadow-2xl">
                        <!-- Left Face (Blue) -->
                        <polygon points="100,100 20,60 20,140 100,180" fill="#3b82f6" stroke="#1e3a8a" stroke-width="2"/>
                        <polygon points="73,86 20,60 20,86 73,113" fill="#60a5fa" stroke="#1e3a8a" stroke-width="1"/>
                        <polygon points="100,100 46,73 46,100 100,126" fill="#2563eb" stroke="#1e3a8a" stroke-width="1"/>
                        <!-- Right Face (Red) -->
                        <polygon points="100,100 180,60 180,140 100,180" fill="#ef4444" stroke="#7f1d1d" stroke-width="2"/>
                        <polygon points="126,86 180,60 180,86 126,113" fill="#f87171" stroke="#7f1d1d" stroke-width="1"/>
                        <polygon points="100,100 153,73 153,100 100,126" fill="#dc2626" stroke="#7f1d1d" stroke-width="1"/>
                        <!-- Top Face (White) -->
                        <polygon points="100,20 20,60 100,100 180,60" fill="#ffffff" stroke="#9ca3af" stroke-width="2"/>
                        <polygon points="100,46 46,73 100,100 153,73" fill="#f9fafb" stroke="#d1d5db" stroke-width="1"/>
                        <polygon points="100,20 73,33 126,60 153,46" fill="#f3f4f6" stroke="#d1d5db" stroke-width="1"/>
                        <!-- Lines for grid effect (simplified) -->
                        <!-- Left -->
                        <line x1="46" y1="46" x2="46" y2="126" stroke="#1e3a8a" stroke-width="2"/>
                        <line x1="73" y1="33" x2="73" y2="153" stroke="#1e3a8a" stroke-width="2"/>
                        <line x1="20" y1="86" x2="100" y2="126" stroke="#1e3a8a" stroke-width="2"/>
                        <line x1="20" y1="113" x2="100" y2="153" stroke="#1e3a8a" stroke-width="2"/>
                        <!-- Right -->
                        <line x1="153" y1="46" x2="153" y2="126" stroke="#7f1d1d" stroke-width="2"/>
                        <line x1="126" y1="33" x2="126" y2="153" stroke="#7f1d1d" stroke-width="2"/>
                        <line x1="180" y1="86" x2="100" y2="126" stroke="#7f1d1d" stroke-width="2"/>
                        <line x1="180" y1="113" x2="100" y2="153" stroke="#7f1d1d" stroke-width="2"/>
                        <!-- Top -->
                        <line x1="46" y1="46" x2="126" y2="86" stroke="#9ca3af" stroke-width="2"/>
                        <line x1="73" y1="33" x2="153" y2="73" stroke="#9ca3af" stroke-width="2"/>
                        <line x1="153" y1="46" x2="73" y2="86" stroke="#9ca3af" stroke-width="2"/>
                        <line x1="126" y1="33" x2="46" y2="73" stroke="#9ca3af" stroke-width="2"/>
                    </svg>
                </div>
            </div>
        </div>
    </section>

    <!-- Tutorial Section -->
    <section id="tutorial" class="py-20 bg-white">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="text-center mb-16">
                <h2 class="text-3xl md:text-4xl font-bold text-darkText mb-4">Belajar Rubik dari Dasar</h2>
                <p class="text-gray-500 max-w-2xl mx-auto text-lg">Ikuti 6 langkah mudah ini untuk menyelesaikan Rubik pertamamu. Klik "Pelajari" untuk menandai progress belajarmu.</p>
            </div>

            <!-- Grid Cards -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                
                <!-- Card 1 -->
                <div class="bg-white rounded-3xl p-8 border border-gray-100 shadow-soft hover:shadow-xl transition-all duration-300 transform hover:-translate-y-2 flex flex-col h-full group">
                    <div class="w-14 h-14 bg-pastel rounded-2xl flex items-center justify-center text-primary text-2xl mb-6 group-hover:scale-110 transition-transform">
                        <i class="fa-solid fa-shapes"></i>
                    </div>
                    <h3 class="text-xl font-bold text-darkText mb-3">1. Pengenalan Rubik 3x3</h3>
                    <p class="text-gray-500 mb-8 flex-grow">Pahami struktur dasar Rubik: Center, Edge, dan Corner agar tidak bingung saat menyusun rumus.</p>
                    <button class="tutorial-btn w-full bg-pastel text-primary hover:bg-primary hover:text-white py-3 rounded-xl font-medium transition" onclick="markProgress(this)">
                        Pelajari
                    </button>
                </div>

                <!-- Card 2 -->
                <div class="bg-white rounded-3xl p-8 border border-gray-100 shadow-soft hover:shadow-xl transition-all duration-300 transform hover:-translate-y-2 flex flex-col h-full group">
                    <div class="w-14 h-14 bg-pastel rounded-2xl flex items-center justify-center text-primary text-2xl mb-6 group-hover:scale-110 transition-transform">
                        <i class="fa-solid fa-font"></i>
                    </div>
                    <h3 class="text-xl font-bold text-darkText mb-3">2. Mengenal Notasi</h3>
                    <p class="text-gray-500 mb-8 flex-grow">Pelajari arti huruf R, U, F, L, D, B dan perbedaannya dengan aksen (') untuk memutar sisi Rubik.</p>
                    <button class="tutorial-btn w-full bg-pastel text-primary hover:bg-primary hover:text-white py-3 rounded-xl font-medium transition" onclick="markProgress(this)">
                        Pelajari
                    </button>
                </div>

                <!-- Card 3 -->
                <div class="bg-white rounded-3xl p-8 border border-gray-100 shadow-soft hover:shadow-xl transition-all duration-300 transform hover:-translate-y-2 flex flex-col h-full group">
                    <div class="w-14 h-14 bg-pastel rounded-2xl flex items-center justify-center text-primary text-2xl mb-6 group-hover:scale-110 transition-transform">
                        <i class="fa-solid fa-plus"></i>
                    </div>
                    <h3 class="text-xl font-bold text-darkText mb-3">3. Membuat Cross</h3>
                    <p class="text-gray-500 mb-8 flex-grow">Langkah pertama penyelesaian: membuat pola silang putih (White Cross) dengan tepian yang sejajar.</p>
                    <button class="tutorial-btn w-full bg-pastel text-primary hover:bg-primary hover:text-white py-3 rounded-xl font-medium transition" onclick="markProgress(this)">
                        Pelajari
                    </button>
                </div>

                <!-- Card 4 -->
                <div class="bg-white rounded-3xl p-8 border border-gray-100 shadow-soft hover:shadow-xl transition-all duration-300 transform hover:-translate-y-2 flex flex-col h-full group">
                    <div class="w-14 h-14 bg-pastel rounded-2xl flex items-center justify-center text-primary text-2xl mb-6 group-hover:scale-110 transition-transform">
                        <i class="fa-solid fa-layer-group"></i>
                    </div>
                    <h3 class="text-xl font-bold text-darkText mb-3">4. First Layer</h3>
                    <p class="text-gray-500 mb-8 flex-grow">Memasukkan sudut (corner) putih ke posisi yang tepat untuk menyelesaikan layer paling bawah.</p>
                    <button class="tutorial-btn w-full bg-pastel text-primary hover:bg-primary hover:text-white py-3 rounded-xl font-medium transition" onclick="markProgress(this)">
                        Pelajari
                    </button>
                </div>

                <!-- Card 5 -->
                <div class="bg-white rounded-3xl p-8 border border-gray-100 shadow-soft hover:shadow-xl transition-all duration-300 transform hover:-translate-y-2 flex flex-col h-full group">
                    <div class="w-14 h-14 bg-pastel rounded-2xl flex items-center justify-center text-primary text-2xl mb-6 group-hover:scale-110 transition-transform">
                        <i class="fa-solid fa-align-center"></i>
                    </div>
                    <h3 class="text-xl font-bold text-darkText mb-3">5. Second Layer</h3>
                    <p class="text-gray-500 mb-8 flex-grow">Menyusun sisi tengah (edge) untuk menyelesaikan dua layer terbawah sekaligus. Hampir selesai!</p>
                    <button class="tutorial-btn w-full bg-pastel text-primary hover:bg-primary hover:text-white py-3 rounded-xl font-medium transition" onclick="markProgress(this)">
                        Pelajari
                    </button>
                </div>

                <!-- Card 6 -->
                <div class="bg-white rounded-3xl p-8 border border-gray-100 shadow-soft hover:shadow-xl transition-all duration-300 transform hover:-translate-y-2 flex flex-col h-full group">
                    <div class="w-14 h-14 bg-pastel rounded-2xl flex items-center justify-center text-primary text-2xl mb-6 group-hover:scale-110 transition-transform">
                        <i class="fa-solid fa-flag-checkered"></i>
                    </div>
                    <h3 class="text-xl font-bold text-darkText mb-3">6. Last Layer</h3>
                    <p class="text-gray-500 mb-8 flex-grow">Orientasi dan permutasi layer terakhir (kuning). Ini adalah tahap akhir menyelesaikan Rubik.</p>
                    <button class="tutorial-btn w-full bg-pastel text-primary hover:bg-primary hover:text-white py-3 rounded-xl font-medium transition" onclick="markProgress(this)">
                        Pelajari
                    </button>
                </div>

            </div>
        </div>
    </section>

    <!-- Formula Section -->
    <section id="rumus" class="py-20 bg-pastel">
        <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="text-center mb-12">
                <h2 class="text-3xl md:text-4xl font-bold text-darkText mb-4">Kamus Rumus Algoritma</h2>
                <p class="text-gray-600 text-lg">Kumpulan rumus krusial untuk mempercepat permainanmu. Klik ikon copy untuk menyalin.</p>
            </div>

            <div class="space-y-4">
                
                <!-- Formula Item -->
                <div class="bg-white p-6 rounded-2xl shadow-sm border border-white hover:border-blue-200 transition flex flex-col md:flex-row items-start md:items-center justify-between gap-4">
                    <div>
                        <h4 class="text-lg font-bold text-darkText">Right Algorithm (Sexy Move)</h4>
                        <p class="text-gray-500 text-sm mt-1">Sangat berguna untuk memasukkan corner dan orientasi dasar.</p>
                    </div>
                    <div class="flex items-center gap-3 w-full md:w-auto bg-pastel px-4 py-2 rounded-xl">
                        <code class="font-mono text-primary font-bold text-lg tracking-wider">R U R' U'</code>
                        <button onclick="copyText('R U R\' U\'')" class="ml-auto md:ml-4 text-gray-400 hover:text-primary transition" title="Copy">
                            <i class="fa-regular fa-copy text-xl"></i>
                        </button>
                    </div>
                </div>

                <!-- Formula Item -->
                <div class="bg-white p-6 rounded-2xl shadow-sm border border-white hover:border-blue-200 transition flex flex-col md:flex-row items-start md:items-center justify-between gap-4">
                    <div>
                        <h4 class="text-lg font-bold text-darkText">Left Algorithm</h4>
                        <p class="text-gray-500 text-sm mt-1">Versi kiri dari Sexy Move, digunakan untuk sisi kiri.</p>
                    </div>
                    <div class="flex items-center gap-3 w-full md:w-auto bg-pastel px-4 py-2 rounded-xl">
                        <code class="font-mono text-primary font-bold text-lg tracking-wider">L' U' L U</code>
                        <button onclick="copyText('L\' U\' L U')" class="ml-auto md:ml-4 text-gray-400 hover:text-primary transition" title="Copy">
                            <i class="fa-regular fa-copy text-xl"></i>
                        </button>
                    </div>
                </div>

                <!-- Formula Item -->
                <div class="bg-white p-6 rounded-2xl shadow-sm border border-white hover:border-blue-200 transition flex flex-col md:flex-row items-start md:items-center justify-between gap-4">
                    <div>
                        <h4 class="text-lg font-bold text-darkText">Sune (OLL Dasar)</h4>
                        <p class="text-gray-500 text-sm mt-1">Digunakan untuk membuat seluruh sisi atas berwarna kuning.</p>
                    </div>
                    <div class="flex items-center gap-3 w-full md:w-auto bg-pastel px-4 py-2 rounded-xl">
                        <code class="font-mono text-primary font-bold text-lg tracking-wider">R U R' U R U2 R'</code>
                        <button onclick="copyText('R U R\' U R U2 R\'')" class="ml-auto md:ml-4 text-gray-400 hover:text-primary transition" title="Copy">
                            <i class="fa-regular fa-copy text-xl"></i>
                        </button>
                    </div>
                </div>

                <!-- Formula Item -->
                <div class="bg-white p-6 rounded-2xl shadow-sm border border-white hover:border-blue-200 transition flex flex-col md:flex-row items-start md:items-center justify-between gap-4">
                    <div>
                        <h4 class="text-lg font-bold text-darkText">T-Permutation (PLL)</h4>
                        <p class="text-gray-500 text-sm mt-1">Menukar 2 corner dan 2 edge yang bersebelahan.</p>
                    </div>
                    <div class="flex items-center gap-3 w-full md:w-auto bg-pastel px-4 py-2 rounded-xl">
                        <code class="font-mono text-primary font-bold text-lg tracking-wider whitespace-nowrap overflow-x-auto">R U R' U' R' F R2 U' R' U' R U R' F'</code>
                        <button onclick="copyText('R U R\' U\' R\' F R2 U\' R\' U\' R U R\' F\'')" class="ml-auto md:ml-4 text-gray-400 hover:text-primary transition shrink-0" title="Copy">
                            <i class="fa-regular fa-copy text-xl"></i>
                        </button>
                    </div>
                </div>

            </div>
        </div>
    </section>

    <!-- Quiz Section -->
    <section id="quiz" class="py-20 bg-white">
        <div class="max-w-3xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="text-center mb-12">
                <div class="w-16 h-16 bg-blue-100 text-primary rounded-full flex items-center justify-center text-3xl mx-auto mb-4">
                    <i class="fa-solid fa-gamepad"></i>
                </div>
                <h2 class="text-3xl md:text-4xl font-bold text-darkText mb-4">Uji Pengetahuanmu!</h2>
                <p class="text-gray-500 text-lg">Kerjakan kuis singkat ini untuk menguji pemahaman dasar Rubik kamu.</p>
            </div>

            <div class="bg-white shadow-[0_8px_30px_rgb(0,0,0,0.06)] rounded-3xl p-6 md:p-10 border border-gray-50">
                <form id="quiz-form" onsubmit="submitQuiz(event)">
                    
                    <!-- Q1 -->
                    <div class="mb-8">
                        <p class="text-lg font-semibold text-darkText mb-4">1. Apa arti dari notasi "R" pada Rubik?</p>
                        <div class="space-y-3">
                            <label class="flex items-center gap-3 p-3 border rounded-xl cursor-pointer hover:bg-pastel transition radio-label">
                                <input type="radio" name="q1" value="a" class="w-4 h-4 text-primary bg-gray-100 border-gray-300 focus:ring-primary">
                                <span class="text-gray-700">Sisi Kanan diputar berlawanan arah jarum jam</span>
                            </label>
                            <label class="flex items-center gap-3 p-3 border rounded-xl cursor-pointer hover:bg-pastel transition radio-label">
                                <input type="radio" name="q1" value="b" class="w-4 h-4 text-primary bg-gray-100 border-gray-300 focus:ring-primary" required>
                                <span class="text-gray-700">Sisi Kanan diputar searah jarum jam</span>
                            </label>
                            <label class="flex items-center gap-3 p-3 border rounded-xl cursor-pointer hover:bg-pastel transition radio-label">
                                <input type="radio" name="q1" value="c" class="w-4 h-4 text-primary bg-gray-100 border-gray-300 focus:ring-primary">
                                <span class="text-gray-700">Sisi Kiri diputar searah jarum jam</span>
                            </label>
                        </div>
                    </div>

                    <!-- Q2 -->
                    <div class="mb-8">
                        <p class="text-lg font-semibold text-darkText mb-4">2. Berapa jumlah potongan Center (pusat) pada Rubik 3x3?</p>
                        <div class="space-y-3">
                            <label class="flex items-center gap-3 p-3 border rounded-xl cursor-pointer hover:bg-pastel transition radio-label">
                                <input type="radio" name="q2" value="a" class="w-4 h-4 text-primary bg-gray-100 border-gray-300 focus:ring-primary" required>
                                <span class="text-gray-700">6 (Enam)</span>
                            </label>
                            <label class="flex items-center gap-3 p-3 border rounded-xl cursor-pointer hover:bg-pastel transition radio-label">
                                <input type="radio" name="q2" value="b" class="w-4 h-4 text-primary bg-gray-100 border-gray-300 focus:ring-primary">
                                <span class="text-gray-700">8 (Delapan)</span>
                            </label>
                            <label class="flex items-center gap-3 p-3 border rounded-xl cursor-pointer hover:bg-pastel transition radio-label">
                                <input type="radio" name="q2" value="c" class="w-4 h-4 text-primary bg-gray-100 border-gray-300 focus:ring-primary">
                                <span class="text-gray-700">12 (Dua Belas)</span>
                            </label>
                        </div>
                    </div>

                    <!-- Q3 -->
                    <div class="mb-8">
                        <p class="text-lg font-semibold text-darkText mb-4">3. F2L adalah singkatan dari?</p>
                        <div class="space-y-3">
                            <label class="flex items-center gap-3 p-3 border rounded-xl cursor-pointer hover:bg-pastel transition radio-label">
                                <input type="radio" name="q3" value="a" class="w-4 h-4 text-primary bg-gray-100 border-gray-300 focus:ring-primary">
                                <span class="text-gray-700">Final Two Layers</span>
                            </label>
                            <label class="flex items-center gap-3 p-3 border rounded-xl cursor-pointer hover:bg-pastel transition radio-label">
                                <input type="radio" name="q3" value="b" class="w-4 h-4 text-primary bg-gray-100 border-gray-300 focus:ring-primary">
                                <span class="text-gray-700">Fast To Learn</span>
                            </label>
                            <label class="flex items-center gap-3 p-3 border rounded-xl cursor-pointer hover:bg-pastel transition radio-label">
                                <input type="radio" name="q3" value="c" class="w-4 h-4 text-primary bg-gray-100 border-gray-300 focus:ring-primary" required>
                                <span class="text-gray-700">First Two Layers</span>
                            </label>
                        </div>
                    </div>

                    <button type="submit" id="submit-quiz-btn" class="w-full bg-primary hover:bg-primaryHover text-white py-4 rounded-xl font-bold text-lg transition shadow-md hover:shadow-lg">
                        Lihat Hasil
                    </button>
                </form>

                <!-- Result Area (Hidden initially) -->
                <div id="quiz-result" class="hidden mt-8 p-6 bg-pastel rounded-2xl text-center border border-blue-200">
                    <h3 class="text-2xl font-bold text-darkText mb-2">Skor Anda: <span id="score-text" class="text-primary"></span></h3>
                    <p id="feedback-text" class="text-gray-600 mb-6"></p>
                    <button onclick="resetQuiz()" class="bg-white border-2 border-primary text-primary hover:bg-blue-50 px-6 py-2 rounded-full font-medium transition">
                        Coba Lagi
                    </button>
                </div>

            </div>
        </div>
    </section>

    <!-- About Section -->
    <section id="about" class="py-20 bg-gradient-to-t from-pastel to-white">
        <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
            <div class="w-20 h-20 bg-white shadow-soft rounded-2xl flex items-center justify-center text-primary text-4xl mx-auto mb-6 transform rotate-12 hover:rotate-0 transition-transform duration-300">
                <i class="fa-solid fa-cube"></i>
            </div>
            <h2 class="text-3xl md:text-4xl font-bold text-darkText mb-6">Tentang RasyaCube</h2>
            <p class="text-gray-600 text-lg md:text-xl leading-relaxed mb-8">
                RasyaCube adalah platform edukasi modern yang didedikasikan untuk membantu siapa saja, dari anak-anak hingga dewasa, belajar menyelesaikan Rubik's Cube dengan mudah. Didesain dengan estetika yang bersih dan navigasi intuitif, kami percaya bahwa logika spasial dapat dipelajari secara menyenangkan tanpa membuat pusing.
            </p>
            <p class="text-primary font-medium text-lg">
                "Setiap masalah yang rumit pasti memiliki algoritma penyelesaian."
            </p>
        </div>
    </section>

    <!-- Footer -->
    <footer class="bg-white border-t border-gray-200 pt-16 pb-8">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex flex-col md:flex-row justify-between items-center md:items-start gap-8 mb-12 text-center md:text-left">
                
                <!-- Logo & Desc -->
                <div>
                    <div class="flex items-center justify-center md:justify-start gap-2 mb-4">
                        <div class="w-8 h-8 bg-primary text-white rounded-lg flex items-center justify-center">
                            <i class="fa-solid fa-cube"></i>
                        </div>
                        <span class="font-bold text-xl text-darkText">Rasya<span class="text-primary">Cube</span></span>
                    </div>
                    <p class="text-gray-500 max-w-xs">Belajar Rubik dari nol hingga mahir. Modern, mudah, dan estetis.</p>
                </div>

                <!-- Links -->
                <div class="flex gap-12">
                    <div>
                        <h4 class="font-semibold text-darkText mb-4">Navigasi</h4>
                        <ul class="space-y-2 text-sm text-gray-500">
                            <li><a href="#home" class="hover:text-primary transition">Home</a></li>
                            <li><a href="#tutorial" class="hover:text-primary transition">Tutorial</a></li>
                            <li><a href="#rumus" class="hover:text-primary transition">Rumus</a></li>
                        </ul>
                    </div>
                    <div>
                        <h4 class="font-semibold text-darkText mb-4">Lainnya</h4>
                        <ul class="space-y-2 text-sm text-gray-500">
                            <li><a href="#quiz" class="hover:text-primary transition">Quiz</a></li>
                            <li><a href="#about" class="hover:text-primary transition">Tentang Kami</a></li>
                        </ul>
                    </div>
                </div>

            </div>
            
            <div class="border-t border-gray-100 pt-8 flex flex-col md:flex-row justify-between items-center gap-4">
                <p class="text-sm text-gray-400">
                    &copy; 2026 RasyaCube. All rights reserved.
                </p>
                <div class="flex gap-4 text-gray-400">
                    <a href="#" class="hover:text-primary transition text-lg"><i class="fa-brands fa-instagram"></i></a>
                    <a href="#" class="hover:text-primary transition text-lg"><i class="fa-brands fa-youtube"></i></a>
                    <a href="#" class="hover:text-primary transition text-lg"><i class="fa-brands fa-tiktok"></i></a>
                </div>
            </div>
        </div>
    </footer>

    <!-- Scripts -->
    <script>
        // Mobile Menu Toggle
        const menuBtn = document.getElementById('mobile-menu-btn');
        const mobileMenu = document.getElementById('mobile-menu');

        menuBtn.addEventListener('click', () => {
            mobileMenu.classList.toggle('hidden');
        });

        // Close mobile menu on link click
        mobileMenu.querySelectorAll('a').forEach(link => {
            link.addEventListener('click', () => {
                mobileMenu.classList.add('hidden');
            });
        });

        // Copy Formula to Clipboard
        function copyText(text) {
            // Use modern clipboard API if available
            if (navigator.clipboard && window.isSecureContext) {
                navigator.clipboard.writeText(text).then(() => {
                    showToast('Rumus "' + text + '" disalin!');
                });
            } else {
                // Fallback for older browsers / non-secure contexts
                let textArea = document.createElement("textarea");
                textArea.value = text;
                textArea.style.position = "fixed";
                textArea.style.left = "-999999px";
                textArea.style.top = "-999999px";
                document.body.appendChild(textArea);
                textArea.focus();
                textArea.select();
                try {
                    document.execCommand('copy');
                    showToast('Rumus "' + text + '" disalin!');
                } catch (err) {
                    showToast('Gagal menyalin rumus.');
                }
                textArea.remove();
            }
        }

        // Custom Toast (No alerts)
        function showToast(message) {
            const toast = document.getElementById('toast');
            const msgEl = document.getElementById('toast-msg');
            msgEl.textContent = message;
            toast.classList.add('show');
            setTimeout(() => {
                toast.classList.remove('show');
            }, 3000);
        }

        // Learning Progress Logic
        let learnedItems = 0;
        const totalItems = 6;
        
        function markProgress(button) {
            if(!button.classList.contains('learned-done')) {
                button.classList.add('learned-done');
                button.innerHTML = '<i class="fa-solid fa-check mr-2"></i> Selesai';
                button.classList.remove('bg-pastel', 'text-primary');
                button.classList.add('bg-green-100', 'text-green-600', 'hover:bg-green-200');
                
                learnedItems++;
                updateProgressBar();
                showToast(`Progress disimpan! (${learnedItems}/${totalItems})`);
            }
        }

        function updateProgressBar() {
            const progressBar = document.getElementById('learning-progress');
            const percentage = (learnedItems / totalItems) * 100;
            progressBar.style.width = percentage + '%';
        }

        // Quiz Logic
        const correctAnswers = {
            q1: 'b', // Sisi Kanan diputar searah jarum jam
            q2: 'a', // 6
            q3: 'c'  // First Two Layers
        };

        function submitQuiz(e) {
            e.preventDefault();
            const form = document.getElementById('quiz-form');
            const formData = new FormData(form);
            
            let score = 0;
            let total = 3;

            // Check answers
            for (let [question, answer] of formData.entries()) {
                if (correctAnswers[question] === answer) {
                    score++;
                }
            }

            // Hide form, show result
            form.classList.add('hidden');
            const resultDiv = document.getElementById('quiz-result');
            const scoreText = document.getElementById('score-text');
            const feedbackText = document.getElementById('feedback-text');
            
            resultDiv.classList.remove('hidden');
            scoreText.textContent = `${score} / ${total}`;

            if (score === 3) {
                feedbackText.textContent = "Sempurna! Kamu sudah memahami dasar Rubik dengan sangat baik.";
                scoreText.className = "text-green-500";
            } else if (score === 2) {
                feedbackText.textContent = "Bagus sekali! Hanya kurang sedikit lagi untuk sempurna.";
                scoreText.className = "text-primary";
            } else {
                feedbackText.textContent = "Jangan menyerah, baca lagi bagian tutorial dan coba kembali!";
                scoreText.className = "text-orange-500";
            }
        }

        function resetQuiz() {
            document.getElementById('quiz-form').reset();
            document.getElementById('quiz-form').classList.remove('hidden');
            document.getElementById('quiz-result').classList.add('hidden');
        }

        // Highlighting radio labels when clicked
        const radioInputs = document.querySelectorAll('input[type="radio"]');
        radioInputs.forEach(input => {
            input.addEventListener('change', function() {
                // reset styling for same group
                const groupName = this.name;
                document.querySelectorAll(`input[name="${groupName}"]`).forEach(inp => {
                    inp.closest('.radio-label').classList.remove('border-primary', 'bg-pastel');
                });
                // style selected
                if(this.checked) {
                    this.closest('.radio-label').classList.add('border-primary', 'bg-pastel');
                }
            });
        });
    </script>
</body>
</html>
