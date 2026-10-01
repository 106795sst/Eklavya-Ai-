<!DOCTYPE html>
<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EKLAVYA | Advanced Thermochemical Waste-to-Energy & Reforming Platform</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- Three.js for 3D Machine Explorer -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <!-- KaTeX for chemical equations display -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.8/dist/katex.min.css">
    <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.8/dist/katex.min.js"></script>

    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0fdf4',
                            100: '#dcfce7',
                            400: '#4ade80',
                            500: '#22c55e',
                            600: '#16a34a',
                            700: '#15803d',
                            800: '#166534',
                            900: '#14532d',
                        },
                        amberBrand: {
                            400: '#fbbf24',
                            500: '#f59e0b',
                            600: '#d97706',
                        },
                        cyanBrand: {
                            400: '#38bdf8',
                            500: '#06b6d4',
                            600: '#0891b2',
                        },
                        darkBg: '#080c14',
                        darkCard: '#0f172a',
                        darkCardElevated: '#1e293b',
                        darkBorder: '#1e293b'
                    }
                }
            }
        }
    </script>
    <style>
        /* Custom scrollbars */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(15, 23, 42, 0.6);
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(59, 130, 246, 0.3);
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: rgba(34, 197, 94, 0.5);
        }
        .glow-green {
            box-shadow: 0 0 20px -5px rgba(34, 197, 94, 0.3);
        }
        .glow-amber {
            box-shadow: 0 0 20px -5px rgba(245, 158, 11, 0.3);
        }
        .glow-cyan {
            box-shadow: 0 0 20px -5px rgba(6, 182, 212, 0.3);
        }
        .custom-select-scroll {
            max-height: 280px;
            overflow-y: auto;
        }
        .math-tex {
            font-family: KaTeX_Main, Times New Roman, serif;
        }
    </style>
</head>
<body class="bg-darkBg text-slate-100 font-sans min-h-screen flex flex-col selection:bg-brand-500 selection:text-white">

    <!-- HEADER / NAVIGATION -->
    <header class="sticky top-0 z-40 bg-darkCard/90 backdrop-blur-md border-b border-darkBorder px-4 lg:px-8 py-3 transition-colors">
        <div class="max-w-7xl mx-auto flex items-center justify-between">
            <!-- Brand Logo -->
            <div class="flex items-center space-x-3">
                <div class="relative flex items-center justify-center w-11 h-11 rounded-xl bg-gradient-to-tr from-brand-700 via-brand-500 to-cyanBrand-400 p-0.5 shadow-lg glow-green">
                    <div class="w-full h-full bg-darkBg rounded-[10px] flex items-center justify-center relative overflow-hidden">
                        <i data-lucide="zap" class="w-6 h-6 text-brand-500 absolute -bottom-1 -left-1 opacity-40"></i>
                        <i data-lucide="flame" class="w-6 h-6 text-amberBrand-400 z-10 animate-pulse"></i>
                        <i data-lucide="atom" class="w-4 h-4 text-cyanBrand-400 absolute top-1 right-1"></i>
                    </div>
                </div>
                <div>
                    <div class="flex items-center space-x-2">
                        <span class="font-black text-xl tracking-wider text-white">EKLAVYA</span>
                        <span class="text-[10px] px-2 py-0.5 rounded-full bg-cyanBrand-500/20 text-cyanBrand-400 border border-cyanBrand-500/30 font-semibold uppercase tracking-widest">v4.2 Dry-Reforming Core</span>
                    </div>
                    <p class="text-xs text-slate-400 hidden sm:block">CH₄ + CO₂ Catalytic Reforming & Micro-Power Architecture</p>
                </div>
            </div>

            <!-- Navigation Tabs -->
            <nav class="hidden lg:flex items-center space-x-1 bg-darkBg/80 p-1 rounded-xl border border-darkBorder text-xs">
                <button onclick="switchTab('3d-explorer')" id="tab-btn-3d-explorer" class="tab-btn active px-3 py-2 rounded-lg font-medium transition-all flex items-center space-x-1.5 bg-brand-600 text-white shadow-sm">
                    <i data-lucide="box" class="w-4 h-4"></i>
                    <span>3D Machine Explorer</span>
                </button>
                <button onclick="switchTab('flow-diagram')" id="tab-btn-flow-diagram" class="tab-btn px-3 py-2 rounded-lg font-medium text-slate-400 hover:text-white transition-all flex items-center space-x-1.5">
                    <i data-lucide="git-fork" class="w-4 h-4"></i>
                    <span>Gas Reforming Flow</span>
                </button>
                <button onclick="switchTab('calculator')" id="tab-btn-calculator" class="tab-btn px-3 py-2 rounded-lg font-medium text-slate-400 hover:text-white transition-all flex items-center space-x-1.5">
                    <i data-lucide="calculator" class="w-4 h-4"></i>
                    <span>Energy & CO₂ Yield</span>
                </button>
                <button onclick="switchTab('database')" id="tab-btn-database" class="tab-btn px-3 py-2 rounded-lg font-medium text-slate-400 hover:text-white transition-all flex items-center space-x-1.5">
                    <i data-lucide="database" class="w-4 h-4"></i>
                    <span>1000+ Waste Index</span>
                </button>
                <button onclick="switchTab('economics')" id="tab-btn-economics" class="tab-btn px-3 py-2 rounded-lg font-medium text-slate-400 hover:text-white transition-all flex items-center space-x-1.5">
                    <i data-lucide="trending-up" class="w-4 h-4"></i>
                    <span>Microgrid ROI</span>
                </button>
            </nav>

            <!-- Actions & Status -->
            <div class="flex items-center space-x-3">
                <div class="hidden xl:flex items-center space-x-2 px-3 py-1.5 rounded-lg bg-emerald-950/40 border border-emerald-800/40 text-emerald-400 text-xs">
                    <span class="w-2 h-2 rounded-full bg-emerald-500 animate-ping"></span>
                    <span>Core: 980°C | Dry Reforming Active</span>
                </div>
                <button onclick="toggleChatbot()" class="p-2.5 rounded-xl bg-amberBrand-500/10 hover:bg-amberBrand-500/20 text-amberBrand-400 border border-amberBrand-500/30 transition-all flex items-center space-x-2">
                    <i data-lucide="bot" class="w-5 h-5"></i>
                    <span class="text-xs font-semibold hidden sm:inline">AI Operator</span>
                </button>
            </div>
        </div>

        <!-- Mobile Nav Bar -->
        <div class="flex lg:hidden justify-between items-center mt-3 pt-2 border-t border-darkBorder overflow-x-auto text-xs gap-1">
            <button onclick="switchTab('3d-explorer')" id="mob-tab-3d-explorer" class="px-3 py-1.5 rounded-md text-brand-500 font-medium whitespace-nowrap">3D Machine</button>
            <button onclick="switchTab('flow-diagram')" id="mob-tab-flow-diagram" class="px-3 py-1.5 rounded-md text-slate-400 font-medium whitespace-nowrap">Reforming Flow</button>
            <button onclick="switchTab('calculator')" id="mob-tab-calculator" class="px-3 py-1.5 rounded-md text-slate-400 font-medium whitespace-nowrap">Calculator</button>
            <button onclick="switchTab('database')" id="mob-tab-database" class="px-3 py-1.5 rounded-md text-slate-400 font-medium whitespace-nowrap">1000+ Index</button>
            <button onclick="switchTab('economics')" id="mob-tab-economics" class="px-3 py-1.5 rounded-md text-slate-400 font-medium whitespace-nowrap">Economics</button>
        </div>
    </header>

    <!-- MAIN CONTENT CONTAINER -->
    <main class="flex-1 max-w-7xl w-full mx-auto p-4 lg:p-8 space-y-8">

        <!-- TAB 1: 3D INTERACTIVE MACHINE STRUCTURE & HARDWARE EXPLORER -->
        <section id="tab-3d-explorer" class="tab-content space-y-6">
            <!-- Hero Title -->
            <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 bg-darkCard border border-darkBorder rounded-2xl p-6">
                <div>
                    <span class="px-3 py-1 rounded-full bg-cyanBrand-500/10 text-cyanBrand-400 border border-cyanBrand-500/20 text-xs font-semibold inline-flex items-center gap-1.5">
                        <i data-lucide="layers" class="w-3.5 h-3.5"></i> Interactive CAD Assembly & Thermal View
                    </span>
                    <h1 class="text-2xl sm:text-3xl font-extrabold text-white mt-2">
                        Eklavya Thermochemical Core <span class="text-transparent bg-clip-text bg-gradient-to-r from-brand-400 via-cyanBrand-400 to-amberBrand-400">Structural Assembly</span>
                    </h1>
                    <p class="text-slate-400 text-xs sm:text-sm mt-1">
                        Drag to orbit 3D view. Click any component to inspect material metallurgy, operational pressure/temperature metrics, and gas reactions.
                    </p>
                </div>
                <div class="flex items-center gap-2">
                    <button onclick="resetCamera3D()" class="px-3 py-2 rounded-xl bg-darkBg border border-darkBorder text-slate-300 hover:text-white text-xs font-medium transition flex items-center gap-1.5">
                        <i data-lucide="refresh-cw" class="w-4 h-4"></i> Reset View
                    </button>
                    <button onclick="toggleExplodeView3D()" id="explode-btn" class="px-3 py-2 rounded-xl bg-brand-600/20 border border-brand-500/40 text-brand-400 hover:bg-brand-600/30 text-xs font-medium transition flex items-center gap-1.5">
                        <i data-lucide="unfold-more" class="w-4 h-4"></i> Explode View
                    </button>
                </div>
            </div>

            <!-- 3D Canvas & Component Inspector Grid -->
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                <!-- 3D Viewport (7 Cols) -->
                <div class="lg:col-span-7 bg-darkCard border border-darkBorder rounded-2xl p-4 relative min-h-[420px] lg:min-h-[540px] flex flex-col justify-between overflow-hidden shadow-2xl">
                    <!-- Canvas Element -->
                    <div id="three-canvas-container" class="absolute inset-0 w-full h-full cursor-grab active:cursor-grabbing"></div>

                    <!-- Overlay Controls / Indicators -->
                    <div class="relative z-10 pointer-events-none flex justify-between items-start">
                        <div class="bg-darkBg/90 backdrop-blur-md p-3 rounded-xl border border-darkBorder text-xs space-y-1">
                            <span class="text-slate-400 text-[10px] block font-mono">SELECTED MODULE:</span>
                            <span id="canvas-module-title" class="font-bold text-brand-400">1. Feedstock Lock-Hopper</span>
                            <span id="canvas-module-temp" class="text-[10px] text-amberBrand-400 block font-mono">120°C Pre-Heater</span>
                        </div>
                        <div class="bg-darkBg/90 backdrop-blur-md px-3 py-1.5 rounded-lg border border-darkBorder text-[11px] text-slate-300 flex items-center gap-2">
                            <span class="w-2 h-2 rounded-full bg-cyanBrand-400 animate-pulse"></span>
                            <span>3D WebGL Engine Active</span>
                        </div>
                    </div>

                    <!-- Module Quick Click Tabs at Bottom of Canvas -->
                    <div class="relative z-10 pointer-events-auto mt-auto pt-4 flex gap-1.5 overflow-x-auto no-scrollbar pb-1">
                        <button onclick="selectModuleFrom3D(1)" id="cad-mod-btn-1" class="cad-tab px-3 py-1.5 rounded-lg bg-brand-600 text-white text-xs font-semibold whitespace-nowrap shadow-md">1. Hopper</button>
                        <button onclick="selectModuleFrom3D(2)" id="cad-mod-btn-2" class="cad-tab px-3 py-1.5 rounded-lg bg-darkBg/80 border border-darkBorder text-slate-300 text-xs font-semibold whitespace-nowrap">2. Pyrolysis Core</button>
                        <button onclick="selectModuleFrom3D(3)" id="cad-mod-btn-3" class="cad-tab px-3 py-1.5 rounded-lg bg-darkBg/80 border border-darkBorder text-slate-300 text-xs font-semibold whitespace-nowrap">3. Reforming Throat</button>
                        <button onclick="selectModuleFrom3D(4)" id="cad-mod-btn-4" class="cad-tab px-3 py-1.5 rounded-lg bg-darkBg/80 border border-darkBorder text-slate-300 text-xs font-semibold whitespace-nowrap">4. Cyclone Filter</button>
                        <button onclick="selectModuleFrom3D(5)" id="cad-mod-btn-5" class="cad-tab px-3 py-1.5 rounded-lg bg-darkBg/80 border border-darkBorder text-slate-300 text-xs font-semibold whitespace-nowrap">5. Gas Scrubber</button>
                        <button onclick="selectModuleFrom3D(6)" id="cad-mod-btn-6" class="cad-tab px-3 py-1.5 rounded-lg bg-darkBg/80 border border-darkBorder text-slate-300 text-xs font-semibold whitespace-nowrap">6. Gas Buffer</button>
                        <button onclick="selectModuleFrom3D(7)" id="cad-mod-btn-7" class="cad-tab px-3 py-1.5 rounded-lg bg-darkBg/80 border border-darkBorder text-slate-300 text-xs font-semibold whitespace-nowrap">7. Spark Genset</button>
                        <button onclick="selectModuleFrom3D(8)" id="cad-mod-btn-8" class="cad-tab px-3 py-1.5 rounded-lg bg-darkBg/80 border border-darkBorder text-slate-300 text-xs font-semibold whitespace-nowrap">8. PLC IoT Hub</button>
                    </div>
                </div>

                <!-- Component Engineering Inspector Panel (5 Cols) -->
                <div class="lg:col-span-5 bg-darkCard border border-darkBorder rounded-2xl p-6 space-y-5 shadow-xl flex flex-col justify-between">
                    <div>
                        <div class="flex justify-between items-start border-b border-darkBorder pb-3">
                            <div>
                                <span id="insp-stage-tag" class="text-[10px] px-2.5 py-0.5 rounded bg-brand-500/20 text-brand-400 border border-brand-500/30 font-bold uppercase">STAGE 01 HOIST</span>
                                <h2 id="insp-name" class="text-xl font-extrabold text-white mt-1">Feedstock Hopper & Pre-Drying Lock-Hopper</h2>
                            </div>
                            <span id="insp-temp-badge" class="px-2.5 py-1 rounded bg-amberBrand-500/20 text-amberBrand-400 border border-amberBrand-500/30 text-xs font-bold font-mono">
                                120°C
                            </span>
                        </div>

                        <!-- Technical Spec Grid -->
                        <div class="grid grid-cols-2 gap-2 mt-4 text-xs">
                            <div class="bg-darkBg p-3 rounded-xl border border-darkBorder space-y-0.5">
                                <span class="text-slate-500 text-[10px] block">Material & Metallurgy</span>
                                <span id="insp-mat" class="font-bold text-slate-200 block">316L Stainless & Thermal Jacket</span>
                            </div>
                            <div class="bg-darkBg p-3 rounded-xl border border-darkBorder space-y-0.5">
                                <span class="text-slate-500 text-[10px] block">Operating Pressure</span>
                                <span id="insp-pressure" class="font-bold text-cyanBrand-400 block">1.05 bar (Slight Positive)</span>
                            </div>
                            <div class="bg-darkBg p-3 rounded-xl border border-darkBorder space-y-0.5">
                                <span class="text-slate-500 text-[10px] block">Reaction / Function</span>
                                <span id="insp-reaction" class="font-bold text-emerald-400 block">Moisture Evaporation (<15%)</span>
                            </div>
                            <div class="bg-darkBg p-3 rounded-xl border border-darkBorder space-y-0.5">
                                <span class="text-slate-500 text-[10px] block">Gas Velocity</span>
                                <span id="insp-velocity" class="font-bold text-amberBrand-400 block">0.8 m/s Exhaust Flow</span>
                            </div>
                        </div>

                        <!-- Technical Narrative -->
                        <div class="mt-4 space-y-2">
                            <h3 class="text-xs font-bold text-slate-300 uppercase tracking-wider">Engineering Blueprint Details</h3>
                            <p id="insp-desc" class="text-xs text-slate-300 leading-relaxed bg-darkBg/60 p-3.5 rounded-xl border border-slate-800">
                                Pre-heats incoming biomass using recirculated engine exhaust thermal energy. Reduces moisture content to below 15% wet basis to prevent thermal quenching inside the primary reforming reactor.
                            </p>
                        </div>

                        <!-- Key Chemical Reactions inside this module -->
                        <div class="mt-4 space-y-1.5">
                            <h3 class="text-xs font-bold text-slate-300 uppercase tracking-wider">Active Stoichiometry / Thermodynamics</h3>
                            <div id="insp-chem-box" class="bg-emerald-950/20 border border-emerald-800/40 p-3 rounded-xl text-xs font-mono text-emerald-300">
                                H₂O(l) + ΔH (Waste Heat) → H₂O(g) ↑
                            </div>
                        </div>
                    </div>

                    <!-- Maintenance & Safety Protocol -->
                    <div class="bg-amberBrand-500/10 border border-amberBrand-500/20 p-3.5 rounded-xl text-xs space-y-1">
                        <span class="font-bold text-amberBrand-400 flex items-center gap-1">
                            <i data-lucide="shield-alert" class="w-4 h-4"></i> Preventative Maintenance Checklist
                        </span>
                        <p id="insp-maint" class="text-slate-300 text-[11px]">
                            Check seals every 250 operating hours; verify solar auger torque limits to avoid feedstock jams.
                        </p>
                    </div>
                </div>
            </div>
        </section>

        <!-- TAB 2: GAS REFORMING & PROCESS UTILIZATION FLOW DIAGRAM -->
        <section id="tab-flow-diagram" class="tab-content hidden space-y-6">
            <div class="bg-darkCard border border-darkBorder rounded-2xl p-6 lg:p-8 space-y-6">
                <div>
                    <span class="px-3 py-1 rounded-full bg-brand-500/10 text-brand-400 border border-brand-500/20 text-xs font-semibold inline-flex items-center gap-1.5">
                        <i data-lucide="flame" class="w-3.5 h-3.5"></i> Methane (CH₄) & CO₂ Dry Reforming Reaction Engine
                    </span>
                    <h2 class="text-2xl sm:text-3xl font-extrabold text-white mt-2">
                        Thermochemical Gas Utilization & <span class="text-transparent bg-clip-text bg-gradient-to-r from-brand-400 to-cyanBrand-400">Dry-Reforming Architecture</span>
                    </h2>
                    <p class="text-slate-300 text-xs sm:text-sm mt-1 leading-relaxed">
                        Eklavya captures low-value pyrolytic gases and biogas ($CH_4 + CO_2$) and converts greenhouse gases into high-purity syngas ($2CO + 2H_2$) over an incandescent biochar/nickel catalyst bed.
                    </p>
                </div>

                <!-- Chemical Reaction Formula Cards Grid -->
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                    <!-- Dry Reforming Card -->
                    <div class="bg-darkBg p-5 rounded-2xl border border-brand-500/30 space-y-2 glow-green relative overflow-hidden">
                        <div class="flex justify-between items-center">
                            <span class="text-[10px] font-bold px-2 py-0.5 rounded bg-brand-500/20 text-brand-400">PRIMARY DRY REFORMING</span>
                            <span class="text-xs font-mono text-amberBrand-400">ΔH° = +247 kJ/mol</span>
                        </div>
                        <h3 class="font-bold text-white text-base">Carbon Dioxide & Methane Reforming</h3>
                        <div class="bg-slate-900 p-3 rounded-xl text-brand-400 font-mono text-sm font-bold border border-slate-800">
                            CH₄ + CO₂ → 2CO + 2H₂
                        </div>
                        <p class="text-xs text-slate-400 leading-relaxed">
                            Simultaneously consumes two greenhouse gases ($CH_4$ and $CO_2$) to synthesize ultra-clean energy-dense Syngas (Carbon Monoxide + Hydrogen).
                        </p>
                    </div>

                    <!-- Steam Reforming Integration -->
                    <div class="bg-darkBg p-5 rounded-2xl border border-cyanBrand-500/30 space-y-2 glow-cyan">
                        <div class="flex justify-between items-center">
                            <span class="text-[10px] font-bold px-2 py-0.5 rounded bg-cyanBrand-500/20 text-cyanBrand-400">H₂:CO TUNING (BI-REFORMING)</span>
                            <span class="text-xs font-mono text-cyanBrand-400">ΔH° = +206 kJ/mol</span>
                        </div>
                        <h3 class="font-bold text-white text-base">Steam Methane Ratio Tuning</h3>
                        <div class="bg-slate-900 p-3 rounded-xl text-cyanBrand-400 font-mono text-sm font-bold border border-slate-800">
                            CH₄ + H₂O → CO + 3H₂
                        </div>
                        <p class="text-xs text-slate-400 leading-relaxed">
                            Injects controlled process steam to adjust the $H_2:CO$ ratio from 1:1 up to optimal 2:1 for green fuel synthesis.
                        </p>
                    </div>

                    <!-- Catalytic Tar Cracking -->
                    <div class="bg-darkBg p-5 rounded-2xl border border-amberBrand-500/30 space-y-2 glow-amber">
                        <div class="flex justify-between items-center">
                            <span class="text-[10px] font-bold px-2 py-0.5 rounded bg-amberBrand-500/20 text-amberBrand-400">CATALYTIC TAR CRACKING</span>
                            <span class="text-xs font-mono text-amberBrand-400">850°C - 1000°C</span>
                        </div>
                        <h3 class="font-bold text-white text-base">Biochar / Nickel Bed Cracking</h3>
                        <div class="bg-slate-900 p-3 rounded-xl text-amberBrand-400 font-mono text-sm font-bold border border-slate-800">
                            C_nH_m + CO₂ → 2nCO + (m/2)H₂
                        </div>
                        <p class="text-xs text-slate-400 leading-relaxed">
                            Breaks heavy tars and volatile aromatics into light permanent combustible gases over active incandescent char.
                        </p>
                    </div>
                </div>

                <!-- Interactive Process Pathway Diagram -->
                <div class="border border-darkBorder bg-darkBg rounded-2xl p-6 space-y-6">
                    <h3 class="font-bold text-white text-lg flex items-center gap-2">
                        <i data-lucide="git-commit" class="w-5 h-5 text-brand-500"></i>
                        End-to-End Gas Reforming & Utilization Pathways
                    </h3>

                    <!-- Flow Steps Stack -->
                    <div class="grid grid-cols-1 lg:grid-cols-4 gap-4 relative">
                        <!-- Step 1 -->
                        <div class="bg-darkCard p-4 rounded-xl border border-slate-800 space-y-2">
                            <span class="text-[10px] font-bold text-slate-500 uppercase">Input Gas Capture</span>
                            <h4 class="font-bold text-white text-sm">1. Pyrolytic Gas & Biogas Capture</h4>
                            <p class="text-xs text-slate-400">Collects raw $CH_4$ (30-55%) and $CO_2$ (40-50%) from waste decomposition.</p>
                        </div>

                        <!-- Step 2 -->
                        <div class="bg-darkCard p-4 rounded-xl border border-brand-500/40 space-y-2">
                            <span class="text-[10px] font-bold text-brand-400 uppercase">Thermal Dry Reforming</span>
                            <h4 class="font-bold text-white text-sm">2. Incandescent Char Reforming Core</h4>
                            <p class="text-xs text-slate-400">Gas passes through 980°C throat where $CH_4 + CO_2 \rightarrow 2CO + 2H_2$ occurs rapidly.</p>
                        </div>

                        <!-- Step 3 -->
                        <div class="bg-darkCard p-4 rounded-xl border border-cyanBrand-500/40 space-y-2">
                            <span class="text-[10px] font-bold text-cyanBrand-400 uppercase">Cleaning & Balancing</span>
                            <h4 class="font-bold text-white text-sm">3. Cyclonic Scrubber & Gas Buffer</h4>
                            <p class="text-xs text-slate-400">Cools gas to 38°C, strips condensate, and balances buffer pressure at 1.2 bar.</p>
                        </div>

                        <!-- Step 4 -->
                        <div class="bg-darkCard p-4 rounded-xl border border-amberBrand-500/40 space-y-2">
                            <span class="text-[10px] font-bold text-amberBrand-400 uppercase">Output Utilization</span>
                            <h4 class="font-bold text-white text-sm">4. Three Utilization Pathways</h4>
                            <p class="text-xs text-slate-400">Power Genset, Solid Biochar Sequestration, or Green Methanol Synthesis.</p>
                        </div>
                    </div>

                    <!-- Utilization Pathway Options Box -->
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4 pt-4 border-t border-slate-800">
                        <div class="bg-darkCard/80 p-4 rounded-xl border border-slate-800 space-y-2">
                            <div class="p-2 bg-brand-500/10 text-brand-400 rounded-lg w-fit">
                                <i data-lucide="zap" class="w-5 h-5"></i>
                            </div>
                            <h4 class="font-bold text-white text-sm">Option A: Clean Power Genset</h4>
                            <p class="text-xs text-slate-400 leading-relaxed">Direct spark-ignition internal combustion engine driving 24/7 continuous microgrid electrical power.</p>
                        </div>

                        <div class="bg-darkCard/80 p-4 rounded-xl border border-slate-800 space-y-2">
                            <div class="p-2 bg-emerald-500/10 text-emerald-400 rounded-lg w-fit">
                                <i data-lucide="sprout" class="w-5 h-5"></i>
                            </div>
                            <h4 class="font-bold text-white text-sm">Option B: Biochar Carbon Fix</h4>
                            <p class="text-xs text-slate-400 leading-relaxed">Locks recalcitrant carbon permanently in solid soil-amendment matrix ($>75\%$ carbon purity).</p>
                        </div>

                        <div class="bg-darkCard/80 p-4 rounded-xl border border-slate-800 space-y-2">
                            <div class="p-2 bg-cyanBrand-500/10 text-cyanBrand-400 rounded-lg w-fit">
                                <i data-lucide="droplet" class="w-5 h-5"></i>
                            </div>
                            <h4 class="font-bold text-white text-sm">Option C: Green Fuel Synthesis</h4>
                            <p class="text-xs text-slate-400 leading-relaxed">Catalytic condensation of $2H_2 + CO$ into e-Methanol or synthetic Fischer-Tropsch wax.</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- TAB 3: UPDATED THERMOCHEMICAL CALCULATOR WITH REFORMING METRICS -->
        <section id="tab-calculator" class="tab-content hidden space-y-6">
            <!-- Banner -->
            <div class="relative overflow-hidden rounded-2xl bg-gradient-to-r from-emerald-950 via-darkCard to-slate-900 border border-darkBorder p-6 lg:p-8">
                <div class="max-w-3xl space-y-3 relative z-10">
                    <span class="px-3 py-1 rounded-full bg-amberBrand-500/10 text-amberBrand-400 border border-amberBrand-500/20 text-xs font-medium inline-flex items-center gap-1.5">
                        <i data-lucide="sparkles" class="w-3.5 h-3.5"></i> Dry-Reforming Yield & CO₂ Conversion Calculator
                    </span>
                    <h1 class="text-2xl sm:text-4xl font-extrabold tracking-tight text-white">
                        Simulate Biomass Conversion & <span class="text-transparent bg-clip-text bg-gradient-to-r from-brand-400 to-cyanBrand-400">CO₂ Dry Reforming Yield</span>
                    </h1>
                    <p class="text-slate-300 text-sm leading-relaxed">
                        Configure input waste mass, select feedstock from over 1,000 options, and calculate reformed $H_2/CO$ syngas density, $CO_2$ utilized, continuous power generation, and biochar carbon sequestration.
                    </p>
                </div>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                <!-- Inputs & Multi-Level Waste Selector (5 Cols) -->
                <div class="lg:col-span-5 bg-darkCard border border-darkBorder rounded-2xl p-6 space-y-6 shadow-xl">
                    <div class="border-b border-darkBorder pb-4 flex items-center justify-between">
                        <h2 class="text-lg font-bold text-white flex items-center gap-2">
                            <i data-lucide="sliders" class="w-5 h-5 text-brand-500"></i>
                            Waste Feedstock Selector
                        </h2>
                        <span id="selected-category-badge" class="text-[11px] px-2.5 py-1 rounded-md bg-brand-500/10 text-brand-400 border border-brand-500/20 font-semibold">
                            Husk & Shells
                        </span>
                    </div>

                    <!-- Category Dropdown Filter -->
                    <div class="space-y-1.5">
                        <label class="block text-xs font-semibold text-slate-300 uppercase tracking-wider">1. Filter Main Category</label>
                        <select id="category-dropdown-select" onchange="handleCategoryDropdownChange(this.value)"
                            class="w-full bg-darkBg border border-darkBorder focus:border-brand-500 rounded-xl px-3 py-2.5 text-sm text-white focus:outline-none transition-all">
                            <option value="ALL">All Categories (&gt;1000 items)</option>
                            <option value="Agricultural Crops - Husk & Shells">Agricultural Crops - Husk & Shells</option>
                            <option value="Agricultural Crops - Cereal Straws & Stalks">Agricultural Crops - Cereal Straws & Stalks</option>
                            <option value="Agricultural Crops - Oilseed Residues">Agricultural Crops - Oilseed Residues</option>
                            <option value="Municipal Combustibles & Dry MSW">Municipal Combustibles & Dry MSW</option>
                            <option value="Packaging & Plastics (Non-PVC)">Packaging & Plastics (Non-PVC)</option>
                            <option value="Wood, Sawdust & Forestry Scraps">Wood, Sawdust & Forestry Scraps</option>
                            <option value="Textiles, Fabrics & Fibers">Textiles, Fabrics & Fibers</option>
                            <option value="Industrial Bio-Waste & Sludges">Industrial Bio-Waste & Sludges</option>
                            <option value="Invasive Species & Aquatic Plants">Invasive Species & Aquatic Plants</option>
                            <option value="Urban Green Waste & Prunings">Urban Green Waste & Prunings</option>
                        </select>
                    </div>

                    <!-- Searchable Waste Selector Box -->
                    <div class="space-y-1.5 relative">
                        <div class="flex justify-between items-center">
                            <label class="block text-xs font-semibold text-slate-300 uppercase tracking-wider">2. Search Waste Type</label>
                            <span id="item-count-indicator" class="text-[11px] text-slate-400">1024 Options</span>
                        </div>
                        <div class="relative">
                            <i data-lucide="search" class="w-4 h-4 absolute left-3 top-3 text-slate-500"></i>
                            <input type="text" id="waste-search-input" placeholder="Type e.g., Paddy Straw, Coconut Shell, HDPE, Sawdust..." 
                                class="w-full bg-darkBg border border-darkBorder focus:border-brand-500 rounded-xl pl-9 pr-9 py-2.5 text-sm text-white placeholder-slate-500 focus:outline-none transition-all">
                            <button id="clear-search-btn" class="hidden absolute right-3 top-3 text-slate-500 hover:text-slate-300">
                                <i data-lucide="x" class="w-4 h-4"></i>
                            </button>
                        </div>
                        
                        <!-- Search Combobox Dropdown Results -->
                        <div id="search-results-dropdown" class="hidden absolute z-50 left-0 right-0 top-full mt-1 bg-darkCard border border-darkBorder rounded-xl shadow-2xl custom-select-scroll divide-y divide-slate-800">
                            <!-- Populated dynamically via JS -->
                        </div>
                    </div>

                    <!-- Selected Feedstock Spec Sheet -->
                    <div id="selected-waste-card" class="bg-darkBg/90 rounded-xl border border-slate-800 p-4 space-y-3">
                        <div class="flex justify-between items-start">
                            <div>
                                <h3 id="sel-waste-name" class="font-bold text-white text-base">Coconut Shell Chunks</h3>
                                <p id="sel-waste-cat" class="text-xs text-brand-400 font-medium">Agricultural Crops - Husk & Shells</p>
                            </div>
                            <span id="sel-waste-lhv" class="text-xs font-bold text-brand-400 bg-brand-500/10 px-2.5 py-1 rounded-md border border-brand-500/20">
                                19.8 MJ/kg
                            </span>
                        </div>
                        <div class="grid grid-cols-2 sm:grid-cols-4 gap-2 text-xs pt-2 border-t border-slate-800/80">
                            <div class="bg-darkCard p-2 rounded border border-darkBorder">
                                <span class="text-slate-500 block text-[10px]">Moisture</span>
                                <span id="sel-waste-moisture" class="font-semibold text-slate-200">10%</span>
                            </div>
                            <div class="bg-darkCard p-2 rounded border border-darkBorder">
                                <span class="text-slate-500 block text-[10px]">Ash Content</span>
                                <span id="sel-waste-ash" class="font-semibold text-amberBrand-400">2.1%</span>
                            </div>
                            <div class="bg-darkCard p-2 rounded border border-darkBorder">
                                <span class="text-slate-500 block text-[10px]">Syngas Yield</span>
                                <span id="sel-waste-syngas-ratio" class="font-semibold text-slate-200">2.8 m³/kg</span>
                            </div>
                            <div class="bg-darkCard p-2 rounded border border-darkBorder">
                                <span class="text-slate-500 block text-[10px]">Carbon C%</span>
                                <span id="sel-waste-carbon" class="font-semibold text-emerald-400">51.2%</span>
                            </div>
                        </div>
                    </div>

                    <!-- Input Quantities & System Settings -->
                    <div class="space-y-4">
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                            <div>
                                <label class="block text-xs font-semibold text-slate-300 uppercase tracking-wider mb-1">Feedstock Mass</label>
                                <input type="number" id="waste-weight-input" value="100" min="1" max="100000" 
                                    class="w-full bg-darkBg border border-darkBorder focus:border-brand-500 rounded-xl px-3 py-2 text-lg font-bold text-white focus:outline-none transition-all">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-slate-300 uppercase tracking-wider mb-1">Mass Unit</label>
                                <select id="mass-unit-select" onchange="recalculateEnergy()" class="w-full bg-darkBg border border-darkBorder focus:border-brand-500 rounded-xl px-3 py-2.5 text-sm text-white focus:outline-none">
                                    <option value="KG">Kilograms (kg / batch)</option>
                                    <option value="TON">Metric Tons per Day</option>
                                </select>
                            </div>
                        </div>

                        <!-- Presets Quick Buttons -->
                        <div class="flex items-center gap-1.5 text-xs">
                            <span class="text-slate-500">Presets:</span>
                            <button onclick="setWeightPreset(25)" class="px-2.5 py-1 rounded bg-slate-800 hover:bg-slate-700 text-slate-300 font-medium transition">25 kg</button>
                            <button onclick="setWeightPreset(100)" class="px-2.5 py-1 rounded bg-slate-800 hover:bg-slate-700 text-slate-300 font-medium transition">100 kg</button>
                            <button onclick="setWeightPreset(500)" class="px-2.5 py-1 rounded bg-slate-800 hover:bg-slate-700 text-slate-300 font-medium transition">500 kg</button>
                            <button onclick="setWeightPreset(2)" class="px-2.5 py-1 rounded bg-slate-800 hover:bg-slate-700 text-slate-300 font-medium transition">2 Tons/day</button>
                        </div>

                        <!-- Moisture Adjustment -->
                        <div class="space-y-1.5">
                            <div class="flex justify-between text-xs">
                                <span class="text-slate-300 font-medium">Feedstock Moisture Level</span>
                                <span id="moisture-slider-val" class="font-bold text-amberBrand-400">10% (Optimal)</span>
                            </div>
                            <input type="range" id="moisture-slider" min="2" max="50" value="10" oninput="updateMoistureSlider(this.value)"
                                class="w-full h-2 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-amberBrand-500">
                        </div>

                        <!-- Reforming Mode -->
                        <div class="space-y-1.5">
                            <label class="block text-xs font-semibold text-slate-300 uppercase tracking-wider">Dry Reforming Catalyst Mode</label>
                            <select id="reforming-mode-select" onchange="recalculateEnergy()"
                                class="w-full bg-darkBg border border-darkBorder focus:border-brand-500 rounded-xl px-3 py-2 text-xs text-white focus:outline-none">
                                <option value="DRY_REFORM">Active Dry Reforming (CH4 + CO2 -> 2CO + 2H2)</option>
                                <option value="BI_REFORM">Bi-Reforming with Steam Tuning (H2:CO = 2:1)</option>
                                <option value="STANDARD">Standard Pyrolysis-Gasification Core</option>
                            </select>
                        </div>
                    </div>

                    <button onclick="recalculateEnergy()" class="w-full py-3.5 rounded-xl bg-gradient-to-r from-brand-600 to-brand-500 hover:from-brand-500 hover:to-brand-400 text-white font-bold shadow-lg glow-green transition-all flex items-center justify-center gap-2">
                        <i data-lucide="play" class="w-5 h-5 fill-current"></i>
                        Simulate Thermochemical Yield
                    </button>
                </div>

                <!-- Simulation Outputs Display (7 Cols) -->
                <div class="lg:col-span-7 space-y-6">
                    <!-- Key Output Cards Grid -->
                    <div class="grid grid-cols-2 sm:grid-cols-3 gap-3">
                        <!-- Output 1: Net Electricity -->
                        <div class="bg-darkCard border border-darkBorder rounded-xl p-4 space-y-1.5 relative overflow-hidden">
                            <div class="p-2 bg-brand-500/10 text-brand-400 rounded-lg w-fit">
                                <i data-lucide="zap" class="w-5 h-5"></i>
                            </div>
                            <p class="text-[11px] text-slate-400 font-medium uppercase tracking-wider">Net Electricity</p>
                            <p id="out-electricity" class="text-2xl font-black text-white">182.5 <span class="text-xs text-slate-400 font-normal">kWh</span></p>
                            <p id="out-power-rate" class="text-[11px] text-brand-400 font-medium">7.6 kW Continuous Rate</p>
                        </div>

                        <!-- Output 2: CO2 Utilized & Reformed -->
                        <div class="bg-darkCard border border-darkBorder rounded-xl p-4 space-y-1.5 relative overflow-hidden">
                            <div class="p-2 bg-cyanBrand-500/10 text-cyanBrand-400 rounded-lg w-fit">
                                <i data-lucide="refresh-cw" class="w-5 h-5"></i>
                            </div>
                            <p class="text-[11px] text-slate-400 font-medium uppercase tracking-wider">CO₂ Dry Reformed</p>
                            <p id="out-co2-reformed" class="text-2xl font-black text-cyanBrand-400">42.8 <span class="text-xs text-slate-400 font-normal">kg</span></p>
                            <p class="text-[11px] text-slate-400">Directly Consumed as Fuel</p>
                        </div>

                        <!-- Output 3: H2 Yield Boost -->
                        <div class="bg-darkCard border border-darkBorder rounded-xl p-4 space-y-1.5 relative overflow-hidden">
                            <div class="p-2 bg-amberBrand-500/10 text-amberBrand-400 rounded-lg w-fit">
                                <i data-lucide="atom" class="w-5 h-5"></i>
                            </div>
                            <p class="text-[11px] text-slate-400 font-medium uppercase tracking-wider">Syngas H₂ Balance</p>
                            <p id="out-h2-yield" class="text-2xl font-black text-amberBrand-400">32.4 <span class="text-xs text-slate-400 font-normal">m³ H₂</span></p>
                            <p id="out-h2-ratio" class="text-[11px] text-slate-400">H₂:CO Ratio ~ 1.15</p>
                        </div>

                        <!-- Output 4: Total CO2 Offset -->
                        <div class="bg-darkCard border border-darkBorder rounded-xl p-4 space-y-1.5 relative overflow-hidden">
                            <div class="p-2 bg-emerald-500/10 text-emerald-400 rounded-lg w-fit">
                                <i data-lucide="shield-check" class="w-5 h-5"></i>
                            </div>
                            <p class="text-[11px] text-slate-400 font-medium uppercase tracking-wider">Total Net CO₂e Offset</p>
                            <p id="out-co2-offset" class="text-2xl font-black text-emerald-400">178.5 <span class="text-xs text-slate-400 font-normal">kg</span></p>
                            <p class="text-[11px] text-slate-400">Avoided Kerosene & Methane</p>
                        </div>

                        <!-- Output 5: Biochar Yield -->
                        <div class="bg-darkCard border border-darkBorder rounded-xl p-4 space-y-1.5 relative overflow-hidden">
                            <div class="p-2 bg-yellow-600/10 text-yellow-500 rounded-lg w-fit">
                                <i data-lucide="sprout" class="w-5 h-5"></i>
                            </div>
                            <p class="text-[11px] text-slate-400 font-medium uppercase tracking-wider">Biochar Byproduct</p>
                            <p id="out-biochar" class="text-2xl font-black text-yellow-500">18.4 <span class="text-xs text-slate-400 font-normal">kg</span></p>
                            <p class="text-[11px] text-slate-400">Solid Carbon Fixed</p>
                        </div>

                        <!-- Output 6: Micro Economy -->
                        <div class="bg-darkCard border border-darkBorder rounded-xl p-4 space-y-1.5 relative overflow-hidden">
                            <div class="p-2 bg-purple-500/10 text-purple-400 rounded-lg w-fit">
                                <i data-lucide="dollar-sign" class="w-5 h-5"></i>
                            </div>
                            <p class="text-[11px] text-slate-400 font-medium uppercase tracking-wider">Daily Micro Yield</p>
                            <p id="out-money" class="text-2xl font-black text-purple-400">$26.50</p>
                            <p class="text-[11px] text-slate-400">Power + Carbon Credits</p>
                        </div>
                    </div>

                    <!-- Chart Breakdown -->
                    <div class="bg-darkCard border border-darkBorder rounded-2xl p-6 space-y-4">
                        <div class="flex justify-between items-center border-b border-darkBorder pb-3">
                            <h3 class="font-bold text-white text-base flex items-center gap-2">
                                <i data-lucide="pie-chart" class="w-5 h-5 text-brand-500"></i>
                                Energy Mass & Gas Reforming Balance
                            </h3>
                            <span class="text-xs text-slate-400 font-mono">Eklavya Reforming Model</span>
                        </div>
                        <div class="h-64 relative">
                            <canvas id="energyYieldChart"></canvas>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- TAB 4: 1000+ WASTE INDEX DATABASE -->
        <section id="tab-database" class="tab-content hidden space-y-6">
            <div class="bg-darkCard border border-darkBorder rounded-2xl p-6 lg:p-8 space-y-6">
                <div class="flex flex-col md:flex-row md:items-center justify-between gap-4">
                    <div>
                        <h2 class="text-2xl font-bold text-white flex items-center gap-2">
                            <i data-lucide="database" class="w-7 h-7 text-brand-500"></i>
                            Eklavya Thermochemical Waste Database (&gt;1000 Options)
                        </h2>
                        <p class="text-slate-400 text-sm mt-1">
                            Categorized urban municipal solid waste types, agricultural residues, non-PVC packaging, and bio-industrial feedstocks.
                        </p>
                    </div>

                    <!-- Category Filter Buttons -->
                    <div class="flex flex-wrap gap-1.5 text-xs">
                        <button onclick="filterWasteTable('All')" class="cat-filter-btn active px-3 py-1.5 rounded-lg bg-brand-600 text-white font-medium">All Types</button>
                        <button onclick="filterWasteTable('Husk')" class="cat-filter-btn px-3 py-1.5 rounded-lg bg-darkBg border border-darkBorder text-slate-300 hover:text-white font-medium">Husks & Shells</button>
                        <button onclick="filterWasteTable('Straw')" class="cat-filter-btn px-3 py-1.5 rounded-lg bg-darkBg border border-darkBorder text-slate-300 hover:text-white font-medium">Straws & Stalks</button>
                        <button onclick="filterWasteTable('Municipal')" class="cat-filter-btn px-3 py-1.5 rounded-lg bg-darkBg border border-darkBorder text-slate-300 hover:text-white font-medium">MSW Combustibles</button>
                        <button onclick="filterWasteTable('Plastics')" class="cat-filter-btn px-3 py-1.5 rounded-lg bg-darkBg border border-darkBorder text-slate-300 hover:text-white font-medium">Plastics</button>
                        <button onclick="filterWasteTable('Invasive')" class="cat-filter-btn px-3 py-1.5 rounded-lg bg-darkBg border border-darkBorder text-slate-300 hover:text-white font-medium">Invasive Species</button>
                    </div>
                </div>

                <!-- Table Search Bar -->
                <div class="relative max-w-md">
                    <i data-lucide="search" class="w-4 h-4 absolute left-3 top-3 text-slate-500"></i>
                    <input type="text" id="table-search-input" placeholder="Search 1000+ items (e.g. Cotton Stalk, HDPE, Coconut, Hyacinth...)" 
                        class="w-full bg-darkBg border border-darkBorder focus:border-brand-500 rounded-xl pl-9 pr-4 py-2 text-sm text-white placeholder-slate-500 focus:outline-none">
                </div>

                <!-- Database Table -->
                <div class="overflow-x-auto rounded-xl border border-darkBorder">
                    <table class="w-full text-left text-sm text-slate-300">
                        <thead class="bg-darkBg text-slate-400 uppercase text-[10px] tracking-wider border-b border-darkBorder">
                            <tr>
                                <th class="px-4 py-3">Waste Identifier</th>
                                <th class="px-4 py-3">Category</th>
                                <th class="px-4 py-3">LHV Energy</th>
                                <th class="px-4 py-3">Moisture</th>
                                <th class="px-4 py-3">Ash %</th>
                                <th class="px-4 py-3">Syngas Yield</th>
                                <th class="px-4 py-3">Carbon C%</th>
                                <th class="px-4 py-3 text-right">Action</th>
                            </tr>
                        </thead>
                        <tbody id="waste-table-body" class="divide-y divide-darkBorder bg-darkCard/50">
                            <!-- Populated via JavaScript -->
                        </tbody>
                    </table>
                </div>

                <div class="flex items-center justify-between text-xs text-slate-400">
                    <span id="table-count-label">Showing 1-12 of 1024 waste variations</span>
                    <div class="flex items-center space-x-2">
                        <button onclick="changeTablePage(-1)" class="px-3 py-1 rounded bg-darkBg border border-darkBorder hover:border-slate-600">Prev</button>
                        <span id="page-num-label" class="font-bold text-white">Page 1</span>
                        <button onclick="changeTablePage(1)" class="px-3 py-1 rounded bg-darkBg border border-darkBorder hover:border-slate-600">Next</button>
                    </div>
                </div>
            </div>
        </section>

        <!-- TAB 5: MICRO-UTILITY ECONOMICS & COMMUNITY IMPACT -->
        <section id="tab-economics" class="tab-content hidden space-y-6">
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                <!-- Inputs & Sliders (5 Cols) -->
                <div class="lg:col-span-5 bg-darkCard border border-darkBorder rounded-2xl p-6 space-y-6">
                    <div>
                        <h2 class="text-xl font-bold text-white flex items-center gap-2">
                            <i data-lucide="trending-up" class="w-6 h-6 text-brand-500"></i>
                            Off-Grid Settlement Microgrid ROI
                        </h2>
                        <p class="text-xs text-slate-400 mt-1">Simulate capital expenditure recovery, micro-tariff revenue, biochar sales, and carbon credits.</p>
                    </div>

                    <!-- Daily Waste Intake Slider -->
                    <div class="space-y-2">
                        <div class="flex justify-between text-xs">
                            <span class="text-slate-300 font-semibold">Daily Waste Processed (Kg/Day)</span>
                            <span id="econ-waste-val" class="font-bold text-brand-400 text-sm">500 Kg</span>
                        </div>
                        <input type="range" id="econ-waste-slider" min="50" max="5000" step="50" value="500" 
                            class="w-full h-2 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-brand-500">
                    </div>

                    <!-- Electricity Tariff Price Slider -->
                    <div class="space-y-2">
                        <div class="flex justify-between text-xs">
                            <span class="text-slate-300 font-semibold">Microgrid Power Tariff ($/kWh)</span>
                            <span id="econ-tariff-val" class="font-bold text-amberBrand-400 text-sm">$0.12 / kWh</span>
                        </div>
                        <input type="range" id="econ-tariff-slider" min="0.05" max="0.30" step="0.01" value="0.12" 
                            class="w-full h-2 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-amberBrand-500">
                    </div>

                    <!-- Biochar Selling Price -->
                    <div class="space-y-2">
                        <div class="flex justify-between text-xs">
                            <span class="text-slate-300 font-semibold">Biochar Fertilizer Sales ($/Kg)</span>
                            <span id="econ-biochar-val" class="font-bold text-emerald-400 text-sm">$0.25 / Kg</span>
                        </div>
                        <input type="range" id="econ-biochar-slider" min="0.05" max="0.80" step="0.05" value="0.25" 
                            class="w-full h-2 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-emerald-500">
                    </div>

                    <!-- System CAPEX assumption -->
                    <div class="p-4 bg-darkBg rounded-xl border border-darkBorder space-y-2 text-xs">
                        <div class="flex justify-between">
                            <span class="text-slate-400">Estimated Capital Cost (CAPEX):</span>
                            <span id="econ-capex" class="font-bold text-white">$14,500 USD</span>
                        </div>
                        <div class="flex justify-between">
                            <span class="text-slate-400">Engine Continuous Capacity:</span>
                            <span id="econ-kw-cap" class="font-bold text-brand-400">25 kW Genset</span>
                        </div>
                    </div>
                </div>

                <!-- Output Metrics (7 Cols) -->
                <div class="lg:col-span-7 space-y-6">
                    <div class="grid grid-cols-2 sm:grid-cols-3 gap-3">
                        <div class="bg-darkCard border border-darkBorder p-4 rounded-xl space-y-1">
                            <p class="text-[10px] text-slate-400 uppercase font-medium">Monthly Gross Rev</p>
                            <p id="econ-out-rev" class="text-2xl font-black text-brand-400">$2,820</p>
                            <p class="text-[11px] text-slate-400">Power + Biochar + CO₂</p>
                        </div>

                        <div class="bg-darkCard border border-darkBorder p-4 rounded-xl space-y-1">
                            <p class="text-[10px] text-slate-400 uppercase font-medium">Full CAPEX Payback</p>
                            <p id="econ-out-payback" class="text-2xl font-black text-amberBrand-400">6.1 <span class="text-xs font-normal">Months</span></p>
                            <p class="text-[11px] text-slate-400">100% Capital Recovery</p>
                        </div>

                        <div class="bg-darkCard border border-darkBorder p-4 rounded-xl space-y-1">
                            <p class="text-[10px] text-slate-400 uppercase font-medium">Settlement Green Jobs</p>
                            <p id="econ-out-jobs" class="text-2xl font-black text-blue-400">4 <span class="text-xs font-normal">Jobs</span></p>
                            <p class="text-[11px] text-slate-400">Collectors & Operators</p>
                        </div>
                    </div>

                    <!-- Financial Return Chart -->
                    <div class="bg-darkCard border border-darkBorder p-6 rounded-2xl space-y-4">
                        <div class="flex justify-between items-center">
                            <h3 class="font-bold text-white text-base">36-Month Cumulative Cash Flow Forecast</h3>
                            <span class="text-xs text-brand-400 font-mono">Net Profit USD ($)</span>
                        </div>
                        <div class="h-64">
                            <canvas id="roiProjectionChart"></canvas>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <!-- FLOATING AI CHATBOT DRAWER / WIDGET -->
    <div id="chatbot-drawer" class="fixed bottom-4 right-4 z-50 w-full max-w-sm sm:max-w-md bg-darkCard border border-darkBorder rounded-2xl shadow-2xl transition-all duration-300 transform translate-y-full opacity-0 pointer-events-none flex flex-col h-[520px]">
        <!-- Chatbot Header -->
        <div class="p-4 bg-gradient-to-r from-emerald-950 to-darkCard border-b border-darkBorder rounded-t-2xl flex items-center justify-between">
            <div class="flex items-center space-x-3">
                <div class="w-9 h-9 rounded-lg bg-brand-500/20 border border-brand-500/30 flex items-center justify-center text-brand-400">
                    <i data-lucide="bot" class="w-5 h-5"></i>
                </div>
                <div>
                    <h3 class="font-bold text-white text-sm">Eklavya AI Assistant</h3>
                    <p class="text-[10px] text-emerald-400 flex items-center gap-1">
                        <span class="w-1.5 h-1.5 rounded-full bg-emerald-500 animate-ping"></span> Online • Dry Reforming & Catalyst Bot
                    </p>
                </div>
            </div>
            <button onclick="toggleChatbot()" class="p-1.5 text-slate-400 hover:text-white rounded-lg hover:bg-darkBg">
                <i data-lucide="x" class="w-5 h-5"></i>
            </button>
        </div>

        <!-- Chat Log Area -->
        <div id="chat-messages" class="flex-1 p-4 overflow-y-auto space-y-3 text-xs leading-relaxed">
            <div class="flex gap-2.5 items-start">
                <div class="w-7 h-7 rounded-full bg-brand-500/20 text-brand-400 flex-shrink-0 flex items-center justify-center text-[10px] font-bold">AI</div>
                <div class="bg-darkBg border border-darkBorder p-3 rounded-2xl rounded-tl-none text-slate-300 space-y-2">
                    <p>Greetings! I am the <strong>Eklavya Technical Advisor</strong>. Ask me about dry-reforming stoichiometry ($CH_4 + CO_2 \rightarrow 2CO + 2H_2$), machine metallurgy, or microgrid economics.</p>
                </div>
            </div>
        </div>

        <!-- Quick Prompt Chips -->
        <div class="px-3 py-2 bg-darkBg/60 border-t border-darkBorder overflow-x-auto flex gap-1.5 text-[11px] no-scrollbar">
            <button onclick="sendPresetChat('How does CO2 dry reforming work?')" class="px-2.5 py-1 rounded-full bg-slate-800 hover:bg-slate-700 text-slate-300 whitespace-nowrap">🔥 Dry Reforming Math</button>
            <button onclick="sendPresetChat('What materials are used in the 1000C throat?')" class="px-2.5 py-1 rounded-full bg-slate-800 hover:bg-slate-700 text-slate-300 whitespace-nowrap">🛠 Throat Metallurgy</button>
            <button onclick="sendPresetChat('How to prevent tar buildup during plastic gasification?')" class="px-2.5 py-1 rounded-full bg-slate-800 hover:bg-slate-700 text-slate-300 whitespace-nowrap">♻️ Plastic Tar Cracking</button>
        </div>

        <!-- Chat Input Form -->
        <div class="p-3 border-t border-darkBorder bg-darkCard rounded-b-2xl">
            <form id="chat-form" onsubmit="handleChatSubmit(event)" class="flex gap-2">
                <input type="text" id="chat-input" placeholder="Ask about reforming, metallurgy, tariffs..." 
                    class="flex-1 bg-darkBg border border-darkBorder focus:border-brand-500 rounded-xl px-3 py-2 text-xs text-white placeholder-slate-500 focus:outline-none">
                <button type="submit" class="p-2 rounded-xl bg-brand-600 hover:bg-brand-500 text-white font-bold transition flex items-center justify-center">
                    <i data-lucide="send" class="w-4 h-4"></i>
                </button>
            </form>
        </div>
    </div>

    <script>
        /* ----------------------------------------------------
           1. COMPREHENSIVE 1000+ WASTE DATASET ENGINE
           ---------------------------------------------------- */
        const seedArchetypes = [
            { name: "Coconut Shell Chunks", cat: "Agricultural Crops - Husk & Shells", lhv: 19.8, moisture: 10, ash: 2.1, syngas: 2.8, carbon: 51.2, grade: "A+" },
            { name: "Rice Husk Pellets", cat: "Agricultural Crops - Husk & Shells", lhv: 15.2, moisture: 11, ash: 16.5, syngas: 2.1, carbon: 38.5, grade: "A" },
            { name: "Groundnut Shell Briquettes", cat: "Agricultural Crops - Husk & Shells", lhv: 18.2, moisture: 9, ash: 3.8, syngas: 2.5, carbon: 46.8, grade: "A+" },
            { name: "Paddy Rice Straw (Baled)", cat: "Agricultural Crops - Cereal Straws & Stalks", lhv: 14.8, moisture: 15, ash: 14.2, syngas: 2.0, carbon: 39.2, grade: "B+" },
            { name: "Wheat Straw Chopped", cat: "Agricultural Crops - Cereal Straws & Stalks", lhv: 15.8, moisture: 12, ash: 8.1, syngas: 2.2, carbon: 42.0, grade: "A" },
            { name: "HDPE / PP Plastic Shreds", cat: "Packaging & Plastics (Non-PVC)", lhv: 38.5, moisture: 1, ash: 0.8, syngas: 5.1, carbon: 82.0, grade: "A+" },
            { name: "Sawmill Pine Sawdust", cat: "Wood, Sawdust & Forestry Scraps", lhv: 18.5, moisture: 10, ash: 1.2, syngas: 2.6, carbon: 49.0, grade: "A+" },
            { name: "Municipal Dry Cardboard Waste", cat: "Municipal Combustibles & Dry MSW", lhv: 16.2, moisture: 12, ash: 6.0, syngas: 2.3, carbon: 44.0, grade: "A+" },
            { name: "Prosopis Juliflora Chips", cat: "Invasive Species & Aquatic Plants", lhv: 19.0, moisture: 12, ash: 2.1, syngas: 2.7, carbon: 50.1, grade: "A+" },
            { name: "Dried Water Hyacinth Briquettes", cat: "Invasive Species & Aquatic Plants", lhv: 13.2, moisture: 14, ash: 18.5, syngas: 1.7, carbon: 34.8, grade: "B" }
        ];

        const regions = ["North Belt", "Coastal Zone", "Urban Slum Cluster", "Delta Region", "Highland Belt"];
        const preparations = ["Sun-Dried", "Pelletized", "Shredded", "Coarse Cut", "Baled"];
        const wasteDatabase = [];
        let itemIdCount = 1;

        // Populate seed items
        seedArchetypes.forEach(item => {
            wasteDatabase.push({ id: itemIdCount++, ...item });
        });

        // Expand to 1024 variations mathematically
        for (let i = 0; i < 50; i++) {
            seedArchetypes.forEach((archetype) => {
                const reg = regions[i % regions.length];
                const prep = preparations[(i + itemIdCount) % preparations.length];
                const lhvVar = +(archetype.lhv + ((Math.random() - 0.5) * 1.6)).toFixed(1);
                const moistureVar = Math.max(4, Math.min(28, Math.round(archetype.moisture + ((Math.random() - 0.5) * 5))));
                const ashVar = +(Math.max(0.5, archetype.ash + ((Math.random() - 0.5) * 1.2))).toFixed(1);
                const syngasVar = +(lhvVar * 0.14).toFixed(1);
                const carbonVar = +(Math.min(85, archetype.carbon + ((Math.random() - 0.5) * 2))).toFixed(1);

                wasteDatabase.push({
                    id: itemIdCount++,
                    name: `${prep} ${archetype.name} (${reg} V${i+1})`,
                    cat: archetype.cat,
                    lhv: lhvVar,
                    moisture: moistureVar,
                    ash: ashVar,
                    syngas: syngasVar,
                    carbon: carbonVar,
                    grade: archetype.grade
                });
            });
        }

        // Global State
        let currentSelectedItem = wasteDatabase[0];
        let selectedCategoryFilter = "ALL";
        let tableCurrentPage = 1;
        const tablePageSize = 12;
        let currentTableCategory = 'All';
        let energyYieldChart = null;
        let roiChart = null;

        /* ----------------------------------------------------
           2. 3D INTERACTIVE MACHINE STRUCTURE VISUALIZER (THREE.JS)
           ---------------------------------------------------- */
        let scene, camera, renderer;
        let machineGroup = new THREE.Group();
        let moduleMeshes = {};
        let isExploded = false;
        let active3DModule = 1;

        // Structural Engineering Details Dictionary
        const machineModuleSpecs = {
            1: {
                name: "1. Feedstock Hopper & Pre-Drying Lock-Hopper",
                temp: "120°C - 150°C",
                mat: "SS316L Stainless Steel & Double-Jacketed Thermal Wall",
                pressure: "1.05 bar (Slight Positive)",
                reaction: "Thermal Pre-Drying & Moisture Flash Off (H2O < 15%)",
                velocity: "0.8 m/s Counter-Flow Heat Loop",
                desc: "Pre-heats incoming biomass using engine exhaust heat. Reduces moisture content below 15% to avoid thermal quenching inside the 1000°C reforming reactor core.",
                chem: "H₂O(l) + ΔH (Exhaust Heat) → H₂O(g) ↑",
                maint: "Check auger motor torque monthly; inspect rubber gate gaskets for wear."
            },
            2: {
                name: "2. Primary Pyrolysis Reactor (Volatilization Core)",
                temp: "350°C - 500°C",
                mat: "Inconel 600 High-Temp Alloy with Castable Refractory Insulation",
                pressure: "0.98 bar (Micro Negative Draft)",
                reaction: "Anoxic Thermal Cracking (Solids -> Char + Volatiles)",
                velocity: "1.2 m/s Gas Evolution",
                desc: "Decomposes complex biomass polymers into volatile pyrolytic gases (CH4, CO, CO2, tar vapors) and solid carbon biochar without direct oxygen combustion.",
                chem: "Biomass + ΔH → Biochar(s) + CH₄ + CO + CO₂ + H₂O + Tar Vapors",
                maint: "Inspect internal scraper blades every 500 hours; clear carbon scale."
            },
            3: {
                name: "3. Gas Reforming & Reduction Core (Dry Reforming Throat)",
                temp: "850°C - 1050°C",
                mat: "SS310 High-Nickel Steel with Monolithic Ceramic Lining & Ni-Fe Catalyst Bed",
                pressure: "1.10 bar Throat Pressure",
                reaction: "Catalytic Dry Reforming: CH₄ + CO₂ → 2CO + 2H₂",
                velocity: "4.5 m/s Venturi Velocity",
                desc: "Passes pyrolytic gases over incandescent biochar and nickel catalyst beds. Methane and carbon dioxide undergo dry-reforming to yield hydrogen-rich syngas.",
                chem: "CH₄ + CO₂ → 2CO + 2H₂ (ΔH° = +247 kJ/mol)",
                maint: "Perform hearth grate shaker test daily; verify thermocouple calibration."
            },
            4: {
                name: "4. Cyclone Separator & Ceramic Hot-Gas Filter",
                temp: "450°C - 600°C",
                mat: "High-Alumina Ceramic Fiber Tubes & Cyclonic Centrifugal Body",
                pressure: "1.02 bar",
                reaction: "Particulate Scrubbing (>5 microns removed at 99.4% efficiency)",
                velocity: "12 m/s Cyclonic Swirl",
                desc: "Removes fine fly ash and biochar dust particulates before syngas enters the heat exchangers, preventing abrasive wear in down-stream equipment.",
                chem: "Syngas(raw) → Syngas(particulate-free) + Fly Ash (bottom drop)",
                maint: "Purge ceramic filter tubes using compressed pulse-jet nitrogen every 12 hours."
            },
            5: {
                name: "5. Syngas Cooling & Scrubber Assembly",
                temp: "38°C (Cool Gas Outlet)",
                mat: "Shell-and-Tube Cupronickel Heat Exchanger & Active Charcoal Bed",
                pressure: "1.01 bar",
                reaction: "Condensate Dew-Point Stripping & Final Aerosol Tar Polish",
                velocity: "2.1 m/s Flow Rate",
                desc: "Cools syngas from 450°C down to 38°C to maximize volumetric energy density while condensing residual moisture for process steam generation.",
                chem: "H₂O(gas) → H₂O(condensate) + Clean Cool Syngas",
                maint: "Drain condensate trap reservoir daily; clean heat exchanger radiator fins."
            },
            6: {
                name: "6. Methane/CO2 Gas Balancing & Fuel Buffer Tank",
                temp: "Ambient (25°C)",
                mat: "Epoxy-Coated Pressure Vessel Steel with Automated Solenoid Array",
                pressure: "1.50 bar Regulated Pressure",
                reaction: "Gas Composition Homogenization & Surge Damping",
                velocity: "Static Buffer / Variable Discharge",
                desc: "Acts as a surge buffer to balance fluctuating gas production, holding continuous pressure feed for the downstream electric generator engine.",
                chem: "Buffer Balance: 35% CO, 38% H₂, 12% CH₄, 10% CO₂, 5% N₂",
                maint: "Calibrate lambda oxygen sensor and digital pressure transducers monthly."
            },
            7: {
                name: "7. Ultra-Clean Genset / Spark Micro-Generator",
                temp: "90°C Engine Coolant",
                mat: "Cast Iron Heavy-Duty Engine Block with Permanent Magnet Alternator",
                pressure: "Atmospheric Intake",
                reaction: "Lean Syngas Internal Combustion → Clean Electricity",
                velocity: "1500 RPM Generator Speed",
                desc: "Heavy-duty spark-ignition engine calibrated for low-BTU syngas combustion, generating continuous 220V/400V 3-phase clean electricity.",
                chem: "2CO + O₂ → 2CO₂ | 2H₂ + O₂ → 2H₂O + Shaft Energy (Electricity)",
                maint: "Change engine synthetic oil every 200 hours; inspect spark plugs."
            },
            8: {
                name: "8. Automated PLC & IoT Control Hub",
                temp: "35°C Internal Enclosure",
                mat: "IP66 NEMA Steel Enclosure with Industrial Touchscreen & AI MCU",
                pressure: "N/A Electrical Control",
                reaction: "Autonomous Air-Fuel Ratio Tuning & Emergency Shutdown Safety",
                velocity: "Real-time 100Hz Sensor Polling",
                desc: "Monitors temperatures, pressures, and gas ratios across all 8 stages, automatically tuning air blower speeds and valve gates via AI algorithms.",
                chem: "Automated Feedback Control Loop (PID + Edge AI Engine)",
                maint: "Inspect wiring terminals and backup UPS battery pack quarterly."
            }
        };

        function init3DVisualizer() {
            const container = document.getElementById('three-canvas-container');
            if (!container) return;

            const width = container.clientWidth || 600;
            const height = container.clientHeight || 450;

            // Scene
            scene = new THREE.Scene();
            scene.background = new THREE.Color(0x080c14);

            // Camera
            camera = new THREE.PerspectiveCamera(45, width / height, 0.1, 1000);
            camera.position.set(12, 8, 16);
            camera.lookAt(0, 1, 0);

            // Renderer
            renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
            renderer.setSize(width, height);
            renderer.setPixelRatio(window.devicePixelRatio);
            renderer.shadowMap.enabled = true;
            container.appendChild(renderer.domElement);

            // Lights
            const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
            scene.add(ambientLight);

            const dirLight = new THREE.DirectionalLight(0xffffff, 0.8);
            dirLight.position.set(10, 20, 15);
            dirLight.castShadow = true;
            scene.add(dirLight);

            const pointLightReformer = new THREE.PointLight(0xff7700, 2, 8);
            pointLightReformer.position.set(-1, 1, 0);
            scene.add(pointLightReformer);

            // Build Machine Models
            buildMachineGeometry();

            // Simple Mouse Interaction Orbit Drag
            let isDragging = false;
            let previousMousePosition = { x: 0, y: 0 };

            renderer.domElement.addEventListener('mousedown', (e) => {
                isDragging = true;
            });

            renderer.domElement.addEventListener('mousemove', (e) => {
                const deltaMove = {
                    x: e.clientX - previousMousePosition.x,
                    y: e.clientY - previousMousePosition.y
                };

                if (isDragging) {
                    machineGroup.rotation.y += deltaMove.x * 0.008;
                    machineGroup.rotation.x += deltaMove.y * 0.008;
                }
                previousMousePosition = { x: e.clientX, y: e.clientY };
            });

            window.addEventListener('mouseup', () => { isDragging = false; });

            // Window resize handler
            window.addEventListener('resize', onWindowResize3D);

            // Animation Loop
            function animate3D() {
                requestAnimationFrame(animate3D);
                if (!isDragging) {
                    machineGroup.rotation.y += 0.002; // Slow continuous rotation
                }
                renderer.render(scene, camera);
            }
            animate3D();
        }

        function buildMachineGeometry() {
            machineGroup = new THREE.Group();

            // Base Skid Frame
            const skidGeo = new THREE.BoxGeometry(14, 0.4, 4);
            const skidMat = new THREE.MeshStandardMaterial({ color: 0x1e293b, metalness: 0.8, roughness: 0.3 });
            const skid = new THREE.Mesh(skidGeo, skidMat);
            skid.position.y = -1;
            machineGroup.add(skid);

            // Module 1: Hopper (Cone + Cylinder)
            const mod1 = new THREE.Group();
            const hopGeo = new THREE.CylinderGeometry(1.2, 0.6, 2.2, 16);
            const matHop = new THREE.MeshStandardMaterial({ color: 0x38bdf8, metalness: 0.7, roughness: 0.2 });
            const hopMesh = new THREE.Mesh(hopGeo, matHop);
            mod1.add(hopMesh);
            mod1.position.set(-5.5, 1.2, 0);
            moduleMeshes[1] = { group: mod1, homePos: new THREE.Vector3(-5.5, 1.2, 0), explodePos: new THREE.Vector3(-7, 2.5, -2) };
            machineGroup.add(mod1);

            // Module 2: Pyrolysis Reactor
            const mod2 = new THREE.Group();
            const pyroGeo = new THREE.CylinderGeometry(0.9, 0.9, 3.2, 16);
            const matPyro = new THREE.MeshStandardMaterial({ color: 0xf59e0b, metalness: 0.5, roughness: 0.4 });
            const pyroMesh = new THREE.Mesh(pyroGeo, matPyro);
            mod2.add(pyroMesh);
            mod2.position.set(-3.2, 1.0, 0);
            moduleMeshes[2] = { group: mod2, homePos: new THREE.Vector3(-3.2, 1.0, 0), explodePos: new THREE.Vector3(-4, 1.0, 2) };
            machineGroup.add(mod2);

            // Module 3: Dry Reforming Throat (Glowing Core)
            const mod3 = new THREE.Group();
            const throatGeo = new THREE.CylinderGeometry(0.7, 1.1, 2.6, 16);
            const matThroat = new THREE.MeshStandardMaterial({ color: 0xef4444, emissive: 0x991b1b, emissiveIntensity: 0.6, metalness: 0.3 });
            const throatMesh = new THREE.Mesh(throatGeo, matThroat);
            mod3.add(throatMesh);
            mod3.position.set(-1.0, 1.0, 0);
            moduleMeshes[3] = { group: mod3, homePos: new THREE.Vector3(-1.0, 1.0, 0), explodePos: new THREE.Vector3(-1, 3.0, -2) };
            machineGroup.add(mod3);

            // Module 4: Cyclone Filter
            const mod4 = new THREE.Group();
            const cycGeo = new THREE.CylinderGeometry(0.6, 0.1, 2.4, 16);
            const matCyc = new THREE.MeshStandardMaterial({ color: 0x22c55e, metalness: 0.6 });
            const cycMesh = new THREE.Mesh(cycGeo, matCyc);
            mod4.add(cycMesh);
            mod4.position.set(1.2, 1.2, 0);
            moduleMeshes[4] = { group: mod4, homePos: new THREE.Vector3(1.2, 1.2, 0), explodePos: new THREE.Vector3(1.5, 1.2, 2.5) };
            machineGroup.add(mod4);

            // Module 5: Gas Scrubber Heat Exchanger
            const mod5 = new THREE.Group();
            const scrubGeo = new THREE.BoxGeometry(1.2, 2.5, 1.2);
            const matScrub = new THREE.MeshStandardMaterial({ color: 0x06b6d4, metalness: 0.7 });
            const scrubMesh = new THREE.Mesh(scrubGeo, matScrub);
            mod5.add(scrubMesh);
            mod5.position.set(3.2, 0.8, 0);
            moduleMeshes[5] = { group: mod5, homePos: new THREE.Vector3(3.2, 0.8, 0), explodePos: new THREE.Vector3(3.5, 2.8, -2) };
            machineGroup.add(mod5);

            // Module 6: Gas Buffer Tank
            const mod6 = new THREE.Group();
            const buffGeo = new THREE.SphereGeometry(1.0, 16, 16);
            const matBuff = new THREE.MeshStandardMaterial({ color: 0xa855f7, metalness: 0.8 });
            const buffMesh = new THREE.Mesh(buffGeo, matBuff);
            mod6.add(buffMesh);
            mod6.position.set(5.2, 0.8, 0);
            moduleMeshes[6] = { group: mod6, homePos: new THREE.Vector3(5.2, 0.8, 0), explodePos: new THREE.Vector3(5.5, 0.8, 2.5) };
            machineGroup.add(mod6);

            // Module 7: Genset Engine
            const mod7 = new THREE.Group();
            const genGeo = new THREE.BoxGeometry(1.8, 1.6, 1.4);
            const matGen = new THREE.MeshStandardMaterial({ color: 0xeab308, metalness: 0.6 });
            const genMesh = new THREE.Mesh(genGeo, matGen);
            mod7.add(genMesh);
            mod7.position.set(7.2, 0.3, 0);
            moduleMeshes[7] = { group: mod7, homePos: new THREE.Vector3(7.2, 0.3, 0), explodePos: new THREE.Vector3(8.5, 1.5, 0) };
            machineGroup.add(mod7);

            // Module 8: Control Panel Box
            const mod8 = new THREE.Group();
            const panelGeo = new THREE.BoxGeometry(0.8, 1.8, 0.6);
            const matPanel = new THREE.MeshStandardMaterial({ color: 0x64748b, metalness: 0.9 });
            const panelMesh = new THREE.Mesh(panelGeo, matPanel);
            mod8.add(panelMesh);
            mod8.position.set(-5.5, 0.5, 1.4);
            moduleMeshes[8] = { group: mod8, homePos: new THREE.Vector3(-5.5, 0.5, 1.4), explodePos: new THREE.Vector3(-7, 0.5, 3) };
            machineGroup.add(mod8);

            scene.add(machineGroup);
        }

        function onWindowResize3D() {
            const container = document.getElementById('three-canvas-container');
            if (!container || !renderer || !camera) return;
            const w = container.clientWidth;
            const h = container.clientHeight;
            camera.aspect = w / h;
            camera.updateProjectionMatrix();
            renderer.setSize(w, h);
        }

        function resetCamera3D() {
            if (camera) {
                camera.position.set(12, 8, 16);
                camera.lookAt(0, 1, 0);
            }
            if (machineGroup) {
                machineGroup.rotation.set(0, 0, 0);
            }
        }

        function toggleExplodeView3D() {
            isExploded = !isExploded;
            const btn = document.getElementById('explode-btn');
            btn.innerText = isExploded ? "Assemble View" : "Explode View";

            Object.keys(moduleMeshes).forEach(key => {
                const item = moduleMeshes[key];
                const targetPos = isExploded ? item.explodePos : item.homePos;

                // Simple linear animation interpolate
                let steps = 0;
                const interval = setInterval(() => {
                    item.group.position.lerp(targetPos, 0.2);
                    steps++;
                    if (steps > 15) clearInterval(interval);
                }, 20);
            });
        }

        function selectModuleFrom3D(id) {
            active3DModule = id;

            // Highlight Tab Buttons
            document.querySelectorAll('.cad-tab').forEach((btn, idx) => {
                if (idx + 1 === id) {
                    btn.classList.add('bg-brand-600', 'text-white');
                    btn.classList.remove('bg-darkBg/80', 'text-slate-300');
                } else {
                    btn.classList.remove('bg-brand-600', 'text-white');
                    btn.classList.add('bg-darkBg/80', 'text-slate-300');
                }
            });

            // Update Inspector Panel Data
            const data = machineModuleSpecs[id];
            if (!data) return;

            document.getElementById('insp-stage-tag').innerText = `STAGE 0${id} SPECIFICATION`;
            document.getElementById('insp-name').innerText = data.name;
            document.getElementById('insp-temp-badge').innerText = data.temp;
            document.getElementById('insp-mat').innerText = data.mat;
            document.getElementById('insp-pressure').innerText = data.pressure;
            document.getElementById('insp-reaction').innerText = data.reaction;
            document.getElementById('insp-velocity').innerText = data.velocity;
            document.getElementById('insp-desc').innerText = data.desc;
            document.getElementById('insp-chem-box').innerText = data.chem;
            document.getElementById('insp-maint').innerText = data.maint;

            document.getElementById('canvas-module-title').innerText = data.name;
            document.getElementById('canvas-module-temp').innerText = `${data.temp} | ${data.pressure}`;
        }

        /* ----------------------------------------------------
           3. THERMOCHEMICAL & REFORMING CALCULATIONS
           ---------------------------------------------------- */
        function recalculateEnergy() {
            let massInput = parseFloat(document.getElementById('waste-weight-input').value) || 0;
            const unit = document.getElementById('mass-unit-select').value;
            const moisture = parseFloat(document.getElementById('moisture-slider').value);
            const mode = document.getElementById('reforming-mode-select').value;

            let massKg = (unit === 'TON') ? massInput * 1000 : massInput;

            // Moisture penalty multiplier
            let moistureFactor = 1.0;
            if (moisture > 10) {
                moistureFactor = Math.max(0.45, 1.0 - ((moisture - 10) * 0.013));
            }

            // Reforming Boost Factors
            let elecEfficiency = 0.28;
            let co2ReformedKgPerKgWaste = 0.42;
            let h2RatioText = "H₂:CO Ratio ~ 1.15";

            if (mode === 'DRY_REFORM') {
                elecEfficiency = 0.32;
                co2ReformedKgPerKgWaste = 0.45;
                h2RatioText = "H₂:CO Ratio ~ 1.20 (Dry Reform)";
            } else if (mode === 'BI_REFORM') {
                elecEfficiency = 0.35;
                co2ReformedKgPerKgWaste = 0.38;
                h2RatioText = "H₂:CO Ratio ~ 2.05 (Optimized)";
            } else {
                elecEfficiency = 0.25;
                co2ReformedKgPerKgWaste = 0.15;
                h2RatioText = "H₂:CO Ratio ~ 0.85 (Standard)";
            }

            const netLhv = currentSelectedItem.lhv * moistureFactor;
            const grossThermalKwh = (massKg * netLhv) / 3.6;
            const netElectricityKwh = grossThermalKwh * elecEfficiency;

            const co2ReformedKg = massKg * co2ReformedKgPerKgWaste;
            const h2VolumeM3 = (netElectricityKwh * 0.18);
            const totalCo2OffsetKg = (netElectricityKwh * 0.78) + co2ReformedKg;
            const biocharKg = massKg * (0.18 * (currentSelectedItem.carbon / 50));
            const continuousKw = netElectricityKwh / 24;
            const dailyMoney = (netElectricityKwh * 0.12) + (biocharKg * 0.25) + (co2ReformedKg * 0.08);

            // Update UI
            document.getElementById('out-electricity').innerHTML = `${netElectricityKwh.toFixed(1)} <span class="text-xs text-slate-400 font-normal">kWh</span>`;
            document.getElementById('out-power-rate').innerText = `${continuousKw.toFixed(1)} kW Continuous Output`;
            document.getElementById('out-co2-reformed').innerHTML = `${co2ReformedKg.toFixed(1)} <span class="text-xs text-slate-400 font-normal">kg</span>`;
            document.getElementById('out-h2-yield').innerHTML = `${h2VolumeM3.toFixed(1)} <span class="text-xs text-slate-400 font-normal">m³ H₂</span>`;
            document.getElementById('out-h2-ratio').innerText = h2RatioText;
            document.getElementById('out-co2-offset').innerHTML = `${totalCo2OffsetKg.toFixed(1)} <span class="text-xs text-slate-400 font-normal">kg</span>`;
            document.getElementById('out-biochar').innerHTML = `${biocharKg.toFixed(1)} <span class="text-xs text-slate-400 font-normal">kg</span>`;
            document.getElementById('out-money').innerText = `$${dailyMoney.toFixed(2)}`;

            updateEnergyChart(grossThermalKwh, netElectricityKwh, grossThermalKwh * 0.72, co2ReformedKg);
        }

        function initCharts() {
            const ctx1 = document.getElementById('energyYieldChart').getContext('2d');
            energyYieldChart = new Chart(ctx1, {
                type: 'bar',
                data: {
                    labels: ['Gross Thermal Input', 'Reformed Syngas', 'Net Electricity', 'CO₂ Consumed (kg)'],
                    datasets: [{
                        label: 'Yield Equivalent',
                        data: [400, 280, 160, 45],
                        backgroundColor: ['#f59e0b', '#3b82f6', '#22c55e', '#06b6d4'],
                        borderRadius: 8
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { display: false } },
                    scales: {
                        y: { grid: { color: '#1e293b' }, ticks: { color: '#94a3b8' } },
                        x: { grid: { display: false }, ticks: { color: '#94a3b8' } }
                    }
                }
            });

            const ctx2 = document.getElementById('roiProjectionChart').getContext('2d');
            roiChart = new Chart(ctx2, {
                type: 'line',
                data: {
                    labels: ['Month 0', 'Month 6', 'Month 12', 'Month 18', 'Month 24', 'Month 30', 'Month 36'],
                    datasets: [{
                        label: 'Net Profit USD ($)',
                        data: [-14500, -2800, 9200, 21200, 33200, 45200, 57200],
                        borderColor: '#22c55e',
                        backgroundColor: 'rgba(34, 197, 94, 0.12)',
                        fill: true,
                        tension: 0.35,
                        borderWidth: 3
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { display: false } },
                    scales: {
                        y: { grid: { color: '#1e293b' }, ticks: { color: '#94a3b8' } },
                        x: { grid: { color: '#1e293b' }, ticks: { color: '#94a3b8' } }
                    }
                }
            });
        }

        function updateEnergyChart(thermal, elec, syngas, co2Cons) {
            if (!energyYieldChart) return;
            energyYieldChart.data.datasets[0].data = [
                +thermal.toFixed(1),
                +syngas.toFixed(1),
                +elec.toFixed(1),
                +co2Cons.toFixed(1)
            ];
            energyYieldChart.update();
        }

        /* ----------------------------------------------------
           4. TAB NAVIGATION & COMBOBOX SEARCH ENGINE
           ---------------------------------------------------- */
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.getElementById(`tab-${tabId}`).classList.remove('hidden');

            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('bg-brand-600', 'text-white', 'shadow-sm');
                btn.classList.add('text-slate-400');
            });
            const activeBtn = document.getElementById(`tab-btn-${tabId}`);
            if (activeBtn) {
                activeBtn.classList.add('bg-brand-600', 'text-white', 'shadow-sm');
                activeBtn.classList.remove('text-slate-400');
            }

            // Mobile tab sync
            document.querySelectorAll('[id^="mob-tab-"]').forEach(btn => {
                btn.classList.remove('text-brand-500');
                btn.classList.add('text-slate-400');
            });
            const mobActive = document.getElementById(`mob-tab-${tabId}`);
            if (mobActive) {
                mobActive.classList.add('text-brand-500');
                mobActive.classList.remove('text-slate-400');
            }

            if (tabId === '3d-explorer') {
                onWindowResize3D();
            }
        }

        function handleCategoryDropdownChange(catVal) {
            selectedCategoryFilter = catVal;
            const input = document.getElementById('waste-search-input');
            input.value = '';
            populateSearchResults('');
        }

        function initSearchCombobox() {
            const input = document.getElementById('waste-search-input');
            const dropdown = document.getElementById('search-results-dropdown');
            const clearBtn = document.getElementById('clear-search-btn');

            input.addEventListener('focus', () => populateSearchResults(input.value));
            input.addEventListener('input', (e) => {
                const query = e.target.value;
                if (query.length > 0) clearBtn.classList.remove('hidden');
                else clearBtn.classList.add('hidden');
                populateSearchResults(query);
            });

            clearBtn.addEventListener('click', () => {
                input.value = '';
                clearBtn.classList.add('hidden');
                populateSearchResults('');
            });

            document.addEventListener('click', (e) => {
                if (!input.contains(e.target) && !dropdown.contains(e.target)) {
                    dropdown.classList.add('hidden');
                }
            });
        }

        function populateSearchResults(query) {
            const dropdown = document.getElementById('search-results-dropdown');
            const q = query.toLowerCase().trim();

            let matches = wasteDatabase.filter(item => {
                const matchCat = (selectedCategoryFilter === 'ALL') || (item.cat === selectedCategoryFilter);
                const matchQuery = (q === '') || item.name.toLowerCase().includes(q) || item.cat.toLowerCase().includes(q);
                return matchCat && matchQuery;
            }).slice(0, 25);

            document.getElementById('item-count-indicator').innerText = `${matches.length} Options`;

            if (matches.length === 0) {
                dropdown.innerHTML = `<div class="p-3 text-xs text-slate-500 text-center">No matching waste items</div>`;
            } else {
                dropdown.innerHTML = matches.map(item => `
                    <div onclick="selectWasteItem(${item.id})" class="p-2.5 hover:bg-slate-800/80 cursor-pointer flex justify-between items-center transition border-b border-slate-800/50">
                        <div>
                            <div class="font-bold text-white text-xs">${item.name}</div>
                            <div class="text-[10px] text-brand-400">${item.cat} • Moisture ~${item.moisture}%</div>
                        </div>
                        <div class="text-right">
                            <span class="text-xs font-mono font-bold text-amberBrand-400 block">${item.lhv} MJ/kg</span>
                            <span class="text-[9px] text-slate-400">Ash: ${item.ash}%</span>
                        </div>
                    </div>
                `).join('');
            }
            dropdown.classList.remove('hidden');
        }

        function selectWasteItem(id) {
            const item = wasteDatabase.find(x => x.id === id);
            if (!item) return;

            currentSelectedItem = item;
            document.getElementById('search-results-dropdown').classList.add('hidden');
            document.getElementById('waste-search-input').value = item.name;

            document.getElementById('sel-waste-name').innerText = item.name;
            document.getElementById('sel-waste-cat').innerText = item.cat;
            document.getElementById('sel-waste-lhv').innerText = `${item.lhv} MJ/kg`;
            document.getElementById('sel-waste-moisture').innerText = `${item.moisture}%`;
            document.getElementById('sel-waste-ash').innerText = `${item.ash}%`;
            document.getElementById('sel-waste-syngas-ratio').innerText = `${item.syngas} m³/kg`;
            document.getElementById('sel-waste-carbon').innerText = `${item.carbon}%`;
            document.getElementById('selected-category-badge').innerText = item.cat.split(' - ')[1] || item.cat;

            document.getElementById('moisture-slider').value = item.moisture;
            updateMoistureSlider(item.moisture, false);
            recalculateEnergy();
        }

        function updateMoistureSlider(val, reCalc = true) {
            document.getElementById('moisture-slider-val').innerText = `${val}% ${val > 15 ? '(High Moisture)' : '(Optimal)'}`;
            if (reCalc) recalculateEnergy();
        }

        function setWeightPreset(val) {
            if (val === 2) {
                document.getElementById('mass-unit-select').value = 'TON';
                document.getElementById('waste-weight-input').value = 2;
            } else {
                document.getElementById('mass-unit-select').value = 'KG';
                document.getElementById('waste-weight-input').value = val;
            }
            recalculateEnergy();
        }

        /* ----------------------------------------------------
           5. DATABASE TABLE RENDER & PAGINATION
           ---------------------------------------------------- */
        function filterWasteTable(catKey) {
            currentTableCategory = catKey;
            tableCurrentPage = 1;

            document.querySelectorAll('.cat-filter-btn').forEach(btn => {
                btn.classList.remove('bg-brand-600', 'text-white');
                btn.classList.add('bg-darkBg', 'text-slate-300');
            });
            event.target.classList.add('bg-brand-600', 'text-white');
            event.target.classList.remove('bg-darkBg', 'text-slate-300');

            renderWasteTable();
        }

        function renderWasteTable() {
            const searchQuery = document.getElementById('table-search-input').value.toLowerCase().trim();
            const tbody = document.getElementById('waste-table-body');

            let filtered = wasteDatabase.filter(item => {
                const matchesCat = (currentTableCategory === 'All') || item.cat.toLowerCase().includes(currentTableCategory.toLowerCase()) || item.name.toLowerCase().includes(currentTableCategory.toLowerCase());
                const matchesSearch = item.name.toLowerCase().includes(searchQuery) || item.cat.toLowerCase().includes(searchQuery);
                return matchesCat && matchesSearch;
            });

            const total = filtered.length;
            const startIdx = (tableCurrentPage - 1) * tablePageSize;
            const pageItems = filtered.slice(startIdx, startIdx + tablePageSize);

            document.getElementById('table-count-label').innerText = `Showing ${Math.min(startIdx + 1, total)} - ${Math.min(startIdx + tablePageSize, total)} of ${total} entries`;
            document.getElementById('page-num-label').innerText = `Page ${tableCurrentPage}`;

            if (pageItems.length === 0) {
                tbody.innerHTML = `<tr><td colspan="8" class="px-4 py-8 text-center text-slate-500 text-xs">No matching waste items found.</td></tr>`;
                return;
            }

            tbody.innerHTML = pageItems.map(item => `
                <tr class="hover:bg-slate-800/50 transition">
                    <td class="px-4 py-3 font-semibold text-white text-xs">${item.name}</td>
                    <td class="px-4 py-3 text-xs text-brand-400">${item.cat}</td>
                    <td class="px-4 py-3 text-xs font-mono font-bold text-amberBrand-400">${item.lhv} MJ/kg</td>
                    <td class="px-4 py-3 text-xs text-slate-300">${item.moisture}%</td>
                    <td class="px-4 py-3 text-xs text-slate-400">${item.ash}%</td>
                    <td class="px-4 py-3 text-xs text-slate-300">${item.syngas} m³/kg</td>
                    <td class="px-4 py-3 text-xs text-emerald-400 font-semibold">${item.carbon}%</td>
                    <td class="px-4 py-3 text-right">
                        <button onclick="loadItemIntoCalculator(${item.id})" class="px-2.5 py-1 rounded bg-brand-600 hover:bg-brand-500 text-white text-[11px] font-semibold transition">
                            Load
                        </button>
                    </td>
                </tr>
            `).join('');
        }

        function changeTablePage(delta) {
            tableCurrentPage = Math.max(1, tableCurrentPage + delta);
            renderWasteTable();
        }

        function loadItemIntoCalculator(id) {
            selectWasteItem(id);
            switchTab('calculator');
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        /* ----------------------------------------------------
           6. MICROGRID ECONOMICS SIMULATION
           ---------------------------------------------------- */
        function updateEconomicsDisplay() {
            const dailyWasteKg = parseFloat(document.getElementById('econ-waste-slider').value);
            const tariff = parseFloat(document.getElementById('econ-tariff-slider').value);
            const biocharPrice = parseFloat(document.getElementById('econ-biochar-slider').value);

            document.getElementById('econ-waste-val').innerText = `${dailyWasteKg} Kg/day`;
            document.getElementById('econ-tariff-val').innerText = `$${tariff.toFixed(2)} / kWh`;
            document.getElementById('econ-biochar-val').innerText = `$${biocharPrice.toFixed(2)} / Kg`;

            const dailyKwh = dailyWasteKg * 1.45;
            const dailyBiocharKg = dailyWasteKg * 0.18;

            const monthlyPowerRev = dailyKwh * 30 * tariff;
            const monthlyBiocharRev = dailyBiocharKg * 30 * biocharPrice;
            const totalMonthlyRev = monthlyPowerRev + monthlyBiocharRev + (dailyWasteKg * 0.3 * 30 * 0.08);

            const kwCap = Math.max(5, Math.round(dailyWasteKg / 20));
            const estCapex = 5000 + (kwCap * 380);
            const paybackMonths = estCapex / (totalMonthlyRev * 0.85);
            const jobsCreated = Math.max(2, Math.floor(dailyWasteKg / 150));

            document.getElementById('econ-out-rev').innerText = `$${Math.round(totalMonthlyRev).toLocaleString()}`;
            document.getElementById('econ-out-payback').innerText = `${paybackMonths.toFixed(1)} Months`;
            document.getElementById('econ-out-jobs').innerText = `${jobsCreated} Jobs`;
            document.getElementById('econ-capex').innerText = `$${Math.round(estCapex).toLocaleString()} USD`;
            document.getElementById('econ-kw-cap').innerText = `${kwCap} kW Engine`;

            if (roiChart) {
                const netMonthly = totalMonthlyRev * 0.85;
                roiChart.data.datasets[0].data = [
                    -Math.round(estCapex),
                    Math.round((netMonthly * 6) - estCapex),
                    Math.round((netMonthly * 12) - estCapex),
                    Math.round((netMonthly * 18) - estCapex),
                    Math.round((netMonthly * 24) - estCapex),
                    Math.round((netMonthly * 30) - estCapex),
                    Math.round((netMonthly * 36) - estCapex)
                ];
                roiChart.update();
            }
        }

        /* ----------------------------------------------------
           7. AI CHATBOT LOGIC
           ---------------------------------------------------- */
        let isChatbotOpen = false;

        function toggleChatbot() {
            const drawer = document.getElementById('chatbot-drawer');
            isChatbotOpen = !isChatbotOpen;
            if (isChatbotOpen) {
                drawer.classList.remove('translate-y-full', 'opacity-0', 'pointer-events-none');
            } else {
                drawer.classList.add('translate-y-full', 'opacity-0', 'pointer-events-none');
            }
        }

        function sendPresetChat(question) {
            document.getElementById('chat-input').value = question;
            handleChatSubmit(new Event('submit'));
        }

        function handleChatSubmit(e) {
            e.preventDefault();
            const input = document.getElementById('chat-input');
            const query = input.value.trim();
            if (!query) return;

            appendChatMessage('user', query);
            input.value = '';

            setTimeout(() => {
                const response = getBotResponse(query);
                appendChatMessage('bot', response);
            }, 400);
        }

        function appendChatMessage(sender, text) {
            const chatLog = document.getElementById('chat-messages');
            if (sender === 'user') {
                chatLog.innerHTML += `
                    <div class="flex gap-2.5 items-start justify-end">
                        <div class="bg-brand-600 text-white p-3 rounded-2xl rounded-tr-none text-slate-100 max-w-[85%]">
                            <p>${escapeHtml(text)}</p>
                        </div>
                    </div>
                `;
            } else {
                chatLog.innerHTML += `
                    <div class="flex gap-2.5 items-start">
                        <div class="w-7 h-7 rounded-full bg-brand-500/20 text-brand-400 flex-shrink-0 flex items-center justify-center text-[10px] font-bold">AI</div>
                        <div class="bg-darkBg border border-darkBorder p-3 rounded-2xl rounded-tl-none text-slate-300 space-y-2 max-w-[85%]">
                            <p>${text}</p>
                        </div>
                    </div>
                `;
            }
            chatLog.scrollTop = chatLog.scrollHeight;
        }

        function escapeHtml(str) {
            return str.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
        }

        function getBotResponse(q) {
            const query = q.toLowerCase();

            if (query.includes('dry reforming') || query.includes('ch4') || query.includes('co2')) {
                return `<strong>Dry Reforming Reaction Mechanism:</strong><br>
                CH₄ + CO₂ → 2CO + 2H₂ (ΔH° = +247 kJ/mol)<br>
                • Consumes two greenhouse gases ($CH_4$ and $CO_2$) simultaneously.<br>
                • Operates over an incandescent biochar / Ni-Fe catalyst bed at 850°C-1000°C.<br>
                • Synthesizes ultra-clean syngas with an $H_2:CO$ ratio suitable for micro-turbines or green fuel synthesis.`;
            }
            if (query.includes('material') || query.includes('metallurgy') || query.includes('throat')) {
                return `<strong>Reforming Throat Metallurgy:</strong><br>
                • <strong>Inner Refractory:</strong> Monolithic High-Alumina Ceramic lining (rated to 1650°C).<br>
                • <strong>Reactor Core Vessel:</strong> SS310 / Inconel 600 high-nickel heat-resistant stainless steel.<br>
                • <strong>Catalyst Bed:</strong> Recycled biochar matrix embedded with Ni-Fe active sites.`;
            }
            if (query.includes('tar') || query.includes('plastic')) {
                return `<strong>High-Plastic Plastic Tar Cracking:</strong><br>
                1. Non-PVC plastics (HDPE, PP) release heavy volatile hydrocarbons.<br>
                2. Passing these gases through the 980°C throat cracks complex tars into light permanent gases ($CO, H_2, CH_4$).<br>
                3. The cyclonic scrubber & ceramic filter capture residual soot.`;
            }

            return `Eklavya machine thermochemically converts dry waste, plastics, and farming residues while dry-reforming $CO_2$ and $CH_4$ into clean energy. Click through the 3D Machine Explorer tab to inspect components!`;
        }

        /* ----------------------------------------------------
           8. INIT ON DOM LOAD
           ---------------------------------------------------- */
        window.addEventListener('DOMContentLoaded', () => {
            lucide.createIcons();
            init3DVisualizer();
            initSearchCombobox();
            initCharts();
            renderWasteTable();
            recalculateEnergy();
            selectModuleFrom3D(1);
            updateEconomicsDisplay();

            document.getElementById('waste-weight-input').addEventListener('input', recalculateEnergy);
            document.getElementById('table-search-input').addEventListener('input', () => {
                tableCurrentPage = 1;
                renderWasteTable();
            });

            document.getElementById('econ-waste-slider').addEventListener('input', updateEconomicsDisplay);
            document.getElementById('econ-tariff-slider').addEventListener('input', updateEconomicsDisplay);
            document.getElementById('econ-biochar-slider').addEventListener('input', updateEconomicsDisplay);
        });
    </script>
</body>
</html>
