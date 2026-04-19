<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BMCC Lost & Found Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://code.iconify.design/3/3.1.0/iconify.min.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Playfair+Display:wght@400;500;600;700&family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        bmcc: {
                            50: '#FDF2F4', 100: '#FCE7EB', 200: '#F9CED6', 300: '#F3A5B5',
                            400: '#E8708A', 500: '#D94466', 600: '#C1274D', 700: '#9E2D50',
                            800: '#7B1E3A', 900: '#5A1530', 950: '#3D0D20',
                        },
                        gold: {
                            50: '#FDF9EF', 100: '#FAF0D5', 200: '#F3DEA8', 300: '#EBC873',
                            400: '#E4B44D', 500: '#C5A55A', 600: '#A6843A', 700: '#86672E',
                            800: '#6E5429', 900: '#5C4625',
                        },
                        surface: { 50: '#FAFAF8', 100: '#F5F3EF', 200: '#EDE9E1', 300: '#DDD7CB' }
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        display: ['Plus Jakarta Sans', 'sans-serif'],
                        serif: ['Playfair Display', 'serif'],
                    },
                }
            }
        }
    </script>
    <style>
        * { scrollbar-width: thin; scrollbar-color: #7B1E3A transparent; }
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-thumb { background: #7B1E3A; border-radius: 3px; }
        @keyframes fadeInUp { from { opacity: 0; transform: translateY(24px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes slideInRight { from { opacity: 0; transform: translateX(40px); } to { opacity: 1; transform: translateX(0); } }
        @keyframes scaleIn { from { opacity: 0; transform: scale(0.9); } to { opacity: 1; transform: scale(1); } }
        @keyframes float { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-8px); } }
        .animate-fadeInUp { animation: fadeInUp 0.6s ease-out forwards; }
        .animate-slideInRight { animation: slideInRight 0.5s ease-out forwards; }
        .animate-scaleIn { animation: scaleIn 0.3s ease-out forwards; }
        .animate-float { animation: float 3s ease-in-out infinite; }
        .stagger-1 { animation-delay: 0.05s; opacity: 0; }
        .stagger-2 { animation-delay: 0.1s; opacity: 0; }
        .stagger-3 { animation-delay: 0.15s; opacity: 0; }
        .stagger-4 { animation-delay: 0.2s; opacity: 0; }
        .stagger-5 { animation-delay: 0.25s; opacity: 0; }
        .stagger-6 { animation-delay: 0.3s; opacity: 0; }
        .stagger-7 { animation-delay: 0.35s; opacity: 0; }
        .stagger-8 { animation-delay: 0.4s; opacity: 0; }
        .glass { background: rgba(255,255,255,0.7); backdrop-filter: blur(16px); -webkit-backdrop-filter: blur(16px); }
        .card-hover { transition: all 0.35s cubic-bezier(0.4, 0, 0.2, 1); }
        .card-hover:hover { transform: translateY(-4px); box-shadow: 0 20px 40px -12px rgba(123,30,58,0.15); }
        .btn-primary { background: linear-gradient(135deg, #7B1E3A, #9E2D50); transition: all 0.3s ease; }
        .btn-primary:hover { background: linear-gradient(135deg, #5A1530, #7B1E3A); transform: translateY(-1px); box-shadow: 0 8px 24px -4px rgba(123,30,58,0.4); }
        .btn-gold { background: linear-gradient(135deg, #C5A55A, #E4B44D); transition: all 0.3s ease; }
        .btn-gold:hover { background: linear-gradient(135deg, #A6843A, #C5A55A); transform: translateY(-1px); box-shadow: 0 8px 24px -4px rgba(197,165,90,0.4); }
        .badge-lost { background: linear-gradient(135deg, #FEF2F2, #FEE2E2); color: #991B1B; border: 1px solid #FECACA; }
        .badge-found { background: linear-gradient(135deg, #F0FDF4, #DCFCE7); color: #166534; border: 1px solid #BBF7D0; }
        .badge-claimed { background: linear-gradient(135deg, #F8FAFC, #F1F5F9); color: #475569; border: 1px solid #E2E8F0; }
        .hero-pattern { background-image: radial-gradient(circle at 20% 50%, rgba(123,30,58,0.06) 0%, transparent 50%), radial-gradient(circle at 80% 20%, rgba(197,165,90,0.08) 0%, transparent 40%), radial-gradient(circle at 60% 80%, rgba(123,30,58,0.04) 0%, transparent 40%); }
        .input-focus:focus { border-color: #7B1E3A; box-shadow: 0 0 0 3px rgba(123,30,58,0.1); }
        .modal-overlay { background: rgba(30,10,18,0.6); backdrop-filter: blur(4px); }
        .toast-container { pointer-events: none; }
        .toast { pointer-events: auto; }
        .category-chip.active { background: #7B1E3A; color: white; border-color: #7B1E3A; }
        .tab-btn.active { color: #7B1E3A; background: white; box-shadow: 0 1px 3px rgba(0,0,0,0.08); }
        .stat-card::before { content: ''; position: absolute; top: 0; left: 0; right: 0; height: 3px; border-radius: 8px 8px 0 0; }
        .stat-card.maroon::before { background: linear-gradient(90deg, #7B1E3A, #9E2D50); }
        .stat-card.green::before { background: linear-gradient(90deg, #166534, #22C55E); }
        .stat-card.gold::before { background: linear-gradient(90deg, #A6843A, #E4B44D); }
        .stat-card.gray::before { background: linear-gradient(90deg, #475569, #94A3B8); }
        .upload-zone { border: 2px dashed #EDE9E1; transition: all 0.25s ease; }
        .upload-zone:hover, .upload-zone.drag-over { border-color: #7B1E3A; background: #FDF2F4; }
        .upload-zone.has-image { border-style: solid; border-color: #BBF7D0; background: #F0FDF4; }
    </style>
</head>
<body class="bg-surface-50 font-sans text-neutral-900 min-h-screen">

    <div id="toastContainer" class="toast-container fixed top-6 right-6 z-[100] flex flex-col gap-3"></div>

    <!-- Navigation -->
    <nav class="fixed top-0 left-0 right-0 z-50 glass border-b border-surface-200/60">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16 lg:h-[72px]">
                <a href="#" class="flex items-center gap-3 group" onclick="navigateTo('home')">
                    <div class="w-10 h-10 lg:w-11 lg:h-11 rounded-xl bg-gradient-to-br from-bmcc-800 to-bmcc-900 flex items-center justify-center shadow-lg shadow-bmcc-800/20 group-hover:shadow-bmcc-800/30 transition-shadow">
                        <span class="iconify text-gold-400 text-xl" data-icon="mdi:school"></span>
                    </div>
                    <div class="flex flex-col">
                        <span class="font-display font-bold text-sm lg:text-base text-bmcc-900 leading-tight tracking-tight">BMCC</span>
                        <span class="text-[10px] lg:text-xs text-neutral-500 font-medium tracking-wide uppercase">Lost & Found</span>
                    </div>
                </a>
                <div class="hidden md:flex items-center gap-1">
                    <button onclick="navigateTo('home')" class="px-4 py-2 rounded-lg text-sm font-medium text-neutral-600 hover:text-bmcc-800 hover:bg-bmcc-50 transition-all">Home</button>
                    <button onclick="navigateTo('browse')" class="px-4 py-2 rounded-lg text-sm font-medium text-neutral-600 hover:text-bmcc-800 hover:bg-bmcc-50 transition-all">Browse Items</button>
                    <button onclick="navigateTo('howItWorks')" class="px-4 py-2 rounded-lg text-sm font-medium text-neutral-600 hover:text-bmcc-800 hover:bg-bmcc-50 transition-all">How It Works</button>
                </div>
                <div class="flex items-center gap-2 lg:gap-3">
                    <button onclick="openModal('reportLost')" class="btn-primary text-white text-xs lg:text-sm font-semibold px-4 lg:px-5 py-2 lg:py-2.5 rounded-xl flex items-center gap-2">
                        <span class="iconify text-base" data-icon="mdi:magnify-close"></span>
                        <span class="hidden sm:inline">Report Lost</span>
                    </button>
                    <button onclick="openModal('reportFound')" class="btn-gold text-bmcc-950 text-xs lg:text-sm font-semibold px-4 lg:px-5 py-2 lg:py-2.5 rounded-xl flex items-center gap-2">
                        <span class="iconify text-base" data-icon="mdi:hand-coin"></span>
                        <span class="hidden sm:inline">Report Found</span>
                    </button>
                    <button onclick="toggleMobileMenu()" class="md:hidden p-2 rounded-lg hover:bg-surface-100 transition-colors">
                        <span id="menuIcon" class="iconify text-xl text-neutral-700" data-icon="mdi:menu"></span>
                    </button>
                </div>
            </div>
        </div>
        <div id="mobileMenu" class="hidden md:hidden border-t border-surface-200/60 bg-white">
            <div class="px-4 py-3 flex flex-col gap-1">
                <button onclick="navigateTo('home'); toggleMobileMenu()" class="px-4 py-2.5 rounded-lg text-sm font-medium text-neutral-700 hover:bg-bmcc-50 hover:text-bmcc-800 transition-all text-left">Home</button>
                <button onclick="navigateTo('browse'); toggleMobileMenu()" class="px-4 py-2.5 rounded-lg text-sm font-medium text-neutral-700 hover:bg-bmcc-50 hover:text-bmcc-800 transition-all text-left">Browse Items</button>
                <button onclick="navigateTo('howItWorks'); toggleMobileMenu()" class="px-4 py-2.5 rounded-lg text-sm font-medium text-neutral-700 hover:bg-bmcc-50 hover:text-bmcc-800 transition-all text-left">How It Works</button>
            </div>
        </div>
    </nav>

    <main class="pt-16 lg:pt-[72px]">
        <!-- Hero -->
        <section id="heroSection" class="hero-pattern relative overflow-hidden">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12 lg:py-20">
                <div class="grid lg:grid-cols-2 gap-10 lg:gap-16 items-center">
                    <div class="animate-fadeInUp">
                        <div class="inline-flex items-center gap-2 px-3 py-1.5 rounded-full bg-bmcc-50 border border-bmcc-100 mb-6">
                            <span class="w-2 h-2 rounded-full bg-green-500 animate-pulse"></span>
                            <span class="text-xs font-semibold text-bmcc-800 tracking-wide">Brihan Maharashtra College of Commerce</span>
                        </div>
                        <h1 class="font-display font-extrabold text-4xl sm:text-5xl lg:text-6xl text-bmcc-950 leading-[1.08] tracking-tight mb-5">
                            Lost Something?
                            <span class="block mt-1 bg-gradient-to-r from-bmcc-800 via-bmcc-600 to-gold-500 bg-clip-text text-transparent">Found Anything?</span>
                        </h1>
                        <p class="text-neutral-500 text-base lg:text-lg leading-relaxed max-w-lg mb-8">
                            The official BMCC Lost & Found portal helps students and staff reunite with their belongings quickly and securely.
                        </p>
                        <div class="relative max-w-xl">
                            <div class="flex items-center bg-white rounded-2xl border border-surface-200 shadow-lg shadow-bmcc-900/5 p-2 focus-within:border-bmcc-300 focus-within:shadow-xl focus-within:shadow-bmcc-900/10 transition-all duration-300">
                                <span class="iconify text-xl text-neutral-400 ml-3 mr-2 flex-shrink-0" data-icon="mdi:magnify"></span>
                                <input id="heroSearch" type="text" placeholder="Search by item name, location, or category..."
                                    class="flex-1 bg-transparent outline-none text-sm lg:text-base text-neutral-800 placeholder:text-neutral-400 py-2.5"
                                    oninput="handleSearch(this.value)" onkeydown="if(event.key==='Enter') navigateTo('browse')">
                                <button onclick="navigateTo('browse')" class="btn-primary text-white text-sm font-semibold px-5 py-2.5 rounded-xl flex-shrink-0">Search</button>
                            </div>
                        </div>
                        <div class="flex items-center gap-6 mt-8">
                            <div>
                                <span class="font-display font-bold text-2xl text-bmcc-800" id="heroStatItems">0</span>
                                <p class="text-xs text-neutral-500 mt-0.5">Items Listed</p>
                            </div>
                            <div class="w-px h-10 bg-surface-200"></div>
                            <div>
                                <span class="font-display font-bold text-2xl text-green-700" id="heroStatReturned">0</span>
                                <p class="text-xs text-neutral-500 mt-0.5">Successfully Returned</p>
                            </div>
                            <div class="w-px h-10 bg-surface-200"></div>
                            <div>
                                <span class="font-display font-bold text-2xl text-gold-600" id="heroStatRate">—</span>
                                <p class="text-xs text-neutral-500 mt-0.5">Return Rate</p>
                            </div>
                        </div>
                    </div>
                    <div class="hidden lg:flex justify-center animate-fadeInUp stagger-2">
                        <div class="relative">
                            <div class="absolute -top-8 -right-8 w-32 h-32 bg-gold-200/40 rounded-full blur-2xl"></div>
                            <div class="absolute -bottom-8 -left-8 w-40 h-40 bg-bmcc-200/40 rounded-full blur-2xl"></div>
                            <div class="relative w-80">
                                <div class="absolute top-4 left-4 w-full h-64 rounded-2xl bg-surface-200/80 rotate-3"></div>
                                <div class="absolute top-2 left-2 w-full h-64 rounded-2xl bg-white/90 shadow-lg -rotate-1"></div>
                                <div class="relative bg-white rounded-2xl shadow-2xl shadow-bmcc-900/10 border border-surface-200 p-6 animate-float">
                                    <div class="flex items-center justify-center flex-col h-52 text-center">
                                        <div class="w-16 h-16 rounded-2xl bg-surface-100 flex items-center justify-center mb-4">
                                            <span class="iconify text-4xl text-surface-300" data-icon="mdi:package-variant"></span>
                                        </div>
                                        <p class="font-display font-semibold text-neutral-700">No items yet</p>
                                        <p class="text-xs text-neutral-400 mt-1">Be the first to report</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="absolute bottom-0 left-0 right-0">
                <svg viewBox="0 0 1440 60" fill="none" class="w-full"><path d="M0 60L48 55C96 50 192 40 288 35C384 30 480 30 576 33.3C672 36.7 768 43.3 864 45C960 46.7 1056 43.3 1152 40C1248 36.7 1344 33.3 1392 31.7L1440 30V60H0Z" fill="#FAFAF8"/></svg>
            </div>
        </section>

        <!-- Stats -->
        <section id="statsSection" class="py-8 lg:py-12 bg-surface-50">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="grid grid-cols-2 lg:grid-cols-4 gap-4 lg:gap-6">
                    <div class="stat-card maroon relative bg-white rounded-2xl p-5 lg:p-6 border border-surface-200 card-hover">
                        <div class="w-10 h-10 rounded-xl bg-bmcc-50 flex items-center justify-center mb-3"><span class="iconify text-xl text-bmcc-700" data-icon="mdi:alert-circle"></span></div>
                        <span class="font-display font-bold text-2xl lg:text-3xl text-bmcc-800" id="statLost">0</span>
                        <p class="text-xs lg:text-sm text-neutral-500 mt-1">Currently Lost</p>
                    </div>
                    <div class="stat-card green relative bg-white rounded-2xl p-5 lg:p-6 border border-surface-200 card-hover">
                        <div class="w-10 h-10 rounded-xl bg-green-50 flex items-center justify-center mb-3"><span class="iconify text-xl text-green-600" data-icon="mdi:check-circle"></span></div>
                        <span class="font-display font-bold text-2xl lg:text-3xl text-green-700" id="statFound">0</span>
                        <p class="text-xs lg:text-sm text-neutral-500 mt-1">Found Items</p>
                    </div>
                    <div class="stat-card gold relative bg-white rounded-2xl p-5 lg:p-6 border border-surface-200 card-hover">
                        <div class="w-10 h-10 rounded-xl bg-gold-50 flex items-center justify-center mb-3"><span class="iconify text-xl text-gold-600" data-icon="mdi:handshake"></span></div>
                        <span class="font-display font-bold text-2xl lg:text-3xl text-gold-700" id="statReturned">0</span>
                        <p class="text-xs lg:text-sm text-neutral-500 mt-1">Returned</p>
                    </div>
                    <div class="stat-card gray relative bg-white rounded-2xl p-5 lg:p-6 border border-surface-200 card-hover">
                        <div class="w-10 h-10 rounded-xl bg-neutral-50 flex items-center justify-center mb-3"><span class="iconify text-xl text-neutral-600" data-icon="mdi:clock-outline"></span></div>
                        <span class="font-display font-bold text-2xl lg:text-3xl text-neutral-700" id="statAvgTime">—</span>
                        <p class="text-xs lg:text-sm text-neutral-500 mt-1">Avg. Return Time</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- Browse Items -->
        <section id="browseSection" class="py-12 lg:py-16 bg-surface-50">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex flex-col sm:flex-row sm:items-end justify-between gap-4 mb-8">
                    <div>
                        <h2 class="font-display font-bold text-2xl lg:text-3xl text-bmcc-950 tracking-tight">Recent Items</h2>
                        <p class="text-neutral-500 text-sm mt-1">Browse and filter to find what you're looking for</p>
                    </div>
                    <button onclick="openModal('reportFound')" class="text-sm font-medium text-bmcc-700 hover:text-bmcc-900 flex items-center gap-1.5 transition-colors self-start sm:self-auto">
                        <span class="iconify" data-icon="mdi:plus-circle"></span> Submit Item
                    </button>
                </div>
                <div class="bg-white rounded-2xl border border-surface-200 p-4 lg:p-5 mb-6 shadow-sm">
                    <div class="flex flex-col lg:flex-row lg:items-center gap-4">
                        <div class="flex-1 relative">
                            <span class="iconify absolute left-3 top-1/2 -translate-y-1/2 text-neutral-400" data-icon="mdi:magnify"></span>
                            <input id="browseSearch" type="text" placeholder="Search items..." class="w-full pl-10 pr-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none transition-all" oninput="handleSearch(this.value)">
                        </div>
                        <div class="flex items-center gap-1 bg-surface-100 rounded-xl p-1">
                            <button onclick="setTab('all')" class="tab-btn active text-xs font-semibold px-4 py-2 rounded-lg transition-all" data-tab="all">All</button>
                            <button onclick="setTab('lost')" class="tab-btn text-xs font-semibold px-4 py-2 rounded-lg text-neutral-500 hover:text-neutral-700 transition-all" data-tab="lost">Lost</button>
                            <button onclick="setTab('found')" class="tab-btn text-xs font-semibold px-4 py-2 rounded-lg text-neutral-500 hover:text-neutral-700 transition-all" data-tab="found">Found</button>
                            <button onclick="setTab('claimed')" class="tab-btn text-xs font-semibold px-4 py-2 rounded-lg text-neutral-500 hover:text-neutral-700 transition-all" data-tab="claimed">Claimed</button>
                        </div>
                        <select id="sortSelect" onchange="handleSort(this.value)" class="text-sm border border-surface-200 rounded-xl px-3 py-2.5 outline-none input-focus bg-white cursor-pointer">
                            <option value="newest">Newest First</option>
                            <option value="oldest">Oldest First</option>
                            <option value="category">By Category</option>
                        </select>
                    </div>
                    <div class="flex flex-wrap gap-2 mt-4 pt-4 border-t border-surface-100">
                        <button onclick="setCategory('all')" class="category-chip active text-xs font-medium px-3 py-1.5 rounded-full border border-surface-200 transition-all hover:border-bmcc-300" data-cat="all">All Categories</button>
                        <button onclick="setCategory('electronics')" class="category-chip text-xs font-medium px-3 py-1.5 rounded-full border border-surface-200 text-neutral-600 transition-all hover:border-bmcc-300" data-cat="electronics"><span class="iconify inline-block mr-1 -mt-0.5" data-icon="mdi:cellphone"></span>Electronics</button>
                        <button onclick="setCategory('cards')" class="category-chip text-xs font-medium px-3 py-1.5 rounded-full border border-surface-200 text-neutral-600 transition-all hover:border-bmcc-300" data-cat="cards"><span class="iconify inline-block mr-1 -mt-0.5" data-icon="mdi:card-account-details"></span>ID/Cards</button>
                        <button onclick="setCategory('bags')" class="category-chip text-xs font-medium px-3 py-1.5 rounded-full border border-surface-200 text-neutral-600 transition-all hover:border-bmcc-300" data-cat="bags"><span class="iconify inline-block mr-1 -mt-0.5" data-icon="mdi:bag-personal"></span>Bags/Wallets</button>
                        <button onclick="setCategory('books')" class="category-chip text-xs font-medium px-3 py-1.5 rounded-full border border-surface-200 text-neutral-600 transition-all hover:border-bmcc-300" data-cat="books"><span class="iconify inline-block mr-1 -mt-0.5" data-icon="mdi:book-open-variant"></span>Books/Notes</button>
                        <button onclick="setCategory('keys')" class="category-chip text-xs font-medium px-3 py-1.5 rounded-full border border-surface-200 text-neutral-600 transition-all hover:border-bmcc-300" data-cat="keys"><span class="iconify inline-block mr-1 -mt-0.5" data-icon="mdi:key"></span>Keys</button>
                        <button onclick="setCategory('clothing')" class="category-chip text-xs font-medium px-3 py-1.5 rounded-full border border-surface-200 text-neutral-600 transition-all hover:border-bmcc-300" data-cat="clothing"><span class="iconify inline-block mr-1 -mt-0.5" data-icon="mdi:tshirt-crew"></span>Clothing</button>
                        <button onclick="setCategory('other')" class="category-chip text-xs font-medium px-3 py-1.5 rounded-full border border-surface-200 text-neutral-600 transition-all hover:border-bmcc-300" data-cat="other"><span class="iconify inline-block mr-1 -mt-0.5" data-icon="mdi:dots-horizontal-circle"></span>Other</button>
                    </div>
                </div>
                <div id="itemsGrid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4 lg:gap-5"></div>
                <div id="emptyState" class="text-center py-16">
                    <div class="w-20 h-20 rounded-2xl bg-surface-100 flex items-center justify-center mx-auto mb-5"><span class="iconify text-5xl text-surface-300" data-icon="mdi:package-variant-remove"></span></div>
                    <h3 class="font-display font-semibold text-lg text-neutral-700">No items reported yet</h3>
                    <p class="text-sm text-neutral-500 mt-1.5 max-w-sm mx-auto">This portal is ready to use. Report a lost or found item to get started.</p>
                    <div class="flex items-center justify-center gap-3 mt-6">
                        <button onclick="openModal('reportLost')" class="btn-primary text-white text-sm font-semibold px-5 py-2.5 rounded-xl flex items-center gap-2"><span class="iconify" data-icon="mdi:magnify-close"></span> Report Lost</button>
                        <button onclick="openModal('reportFound')" class="btn-gold text-bmcc-950 text-sm font-semibold px-5 py-2.5 rounded-xl flex items-center gap-2"><span class="iconify" data-icon="mdi:hand-coin"></span> Report Found</button>
                    </div>
                </div>
                <div id="loadMoreContainer" class="hidden text-center mt-8">
                    <button class="text-sm font-medium text-bmcc-700 hover:text-bmcc-900 border border-bmcc-200 hover:border-bmcc-400 px-6 py-2.5 rounded-xl transition-all">Load More Items</button>
                </div>
            </div>
        </section>

        <!-- How It Works -->
        <section id="howItWorksSection" class="py-16 lg:py-24 bg-white relative overflow-hidden">
            <div class="absolute inset-0 hero-pattern opacity-50"></div>
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative">
                <div class="text-center max-w-2xl mx-auto mb-14">
                    <div class="inline-flex items-center gap-2 px-3 py-1.5 rounded-full bg-gold-50 border border-gold-200 mb-4">
                        <span class="iconify text-gold-600 text-sm" data-icon="mdi:lightbulb-on"></span>
                        <span class="text-xs font-semibold text-gold-700 tracking-wide">Simple Process</span>
                    </div>
                    <h2 class="font-display font-bold text-2xl lg:text-4xl text-bmcc-950 tracking-tight">How It Works</h2>
                    <p class="text-neutral-500 mt-3">Three simple steps to report or recover your belongings</p>
                </div>
                <div class="grid md:grid-cols-3 gap-8 lg:gap-12">
                    <div class="text-center group">
                        <div class="relative mx-auto w-20 h-20 rounded-2xl bg-gradient-to-br from-bmcc-50 to-bmcc-100 flex items-center justify-center mb-6 group-hover:scale-110 transition-transform duration-300">
                            <span class="iconify text-3xl text-bmcc-700" data-icon="mdi:clipboard-text-plus"></span>
                            <span class="absolute -top-2 -right-2 w-7 h-7 rounded-full bg-bmcc-800 text-white text-xs font-bold flex items-center justify-center shadow-lg">1</span>
                        </div>
                        <h3 class="font-display font-bold text-lg text-bmcc-950 mb-2">Report It</h3>
                        <p class="text-sm text-neutral-500 leading-relaxed">Fill out a quick form with item details, upload a photo, and add your contact information.</p>
                    </div>
                    <div class="text-center group">
                        <div class="relative mx-auto w-20 h-20 rounded-2xl bg-gradient-to-br from-gold-50 to-gold-100 flex items-center justify-center mb-6 group-hover:scale-110 transition-transform duration-300">
                            <span class="iconify text-3xl text-gold-600" data-icon="mdi:bell-ring"></span>
                            <span class="absolute -top-2 -right-2 w-7 h-7 rounded-full bg-gold-600 text-white text-xs font-bold flex items-center justify-center shadow-lg">2</span>
                        </div>
                        <h3 class="font-display font-bold text-lg text-bmcc-950 mb-2">Get Matched</h3>
                        <p class="text-sm text-neutral-500 leading-relaxed">Our system automatically matches lost and found items and notifies both parties.</p>
                    </div>
                    <div class="text-center group">
                        <div class="relative mx-auto w-20 h-20 rounded-2xl bg-gradient-to-br from-green-50 to-green-100 flex items-center justify-center mb-6 group-hover:scale-110 transition-transform duration-300">
                            <span class="iconify text-3xl text-green-600" data-icon="mdi:handshake-angle"></span>
                            <span class="absolute -top-2 -right-2 w-7 h-7 rounded-full bg-green-600 text-white text-xs font-bold flex items-center justify-center shadow-lg">3</span>
                        </div>
                        <h3 class="font-display font-bold text-lg text-bmcc-950 mb-2">Recover It</h3>
                        <p class="text-sm text-neutral-500 leading-relaxed">Verify ownership at the college office and collect your item. It's that simple!</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- Collection Office -->
        <section class="py-12 lg:py-16 bg-surface-50">
            <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="bg-gradient-to-br from-bmcc-800 to-bmcc-950 rounded-3xl p-8 lg:p-12 text-white relative overflow-hidden">
                    <div class="absolute top-0 right-0 w-64 h-64 bg-gold-400/10 rounded-full blur-3xl -translate-y-1/2 translate-x-1/2"></div>
                    <div class="absolute bottom-0 left-0 w-48 h-48 bg-bmcc-600/20 rounded-full blur-3xl translate-y-1/2 -translate-x-1/2"></div>
                    <div class="relative">
                        <div class="flex items-center gap-3 mb-5">
                            <div class="w-10 h-10 rounded-xl bg-white/10 flex items-center justify-center"><span class="iconify text-xl text-gold-400" data-icon="mdi:shield-check"></span></div>
                            <h3 class="font-display font-bold text-xl lg:text-2xl">Collection Office</h3>
                        </div>
                        <p class="text-bmcc-200 text-sm lg:text-base leading-relaxed mb-6">All verified items can be collected from the <strong class="text-white">Student Welfare Office</strong>, located on the Ground Floor of the Main Building. Office hours: <strong class="text-white">10:00 AM – 4:00 PM</strong> (Monday to Saturday). Please carry your BMCC ID card for verification.</p>
                        <div class="flex flex-wrap gap-4">
                            <div class="flex items-center gap-2 text-sm text-bmcc-200"><span class="iconify text-gold-400" data-icon="mdi:map-marker"></span>Main Building, Ground Floor</div>
                            <div class="flex items-center gap-2 text-sm text-bmcc-200"><span class="iconify text-gold-400" data-icon="mdi:clock-outline"></span>10 AM – 4 PM, Mon–Sat</div>
                            <div class="flex items-center gap-2 text-sm text-bmcc-200"><span class="iconify text-gold-400" data-icon="mdi:phone"></span>020-2553 1531</div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <!-- Footer -->
    <footer class="bg-bmcc-950 text-white">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12 lg:py-16">
            <div class="grid md:grid-cols-3 gap-10 lg:gap-16">
                <div>
                    <div class="flex items-center gap-3 mb-4">
                        <div class="w-10 h-10 rounded-xl bg-white/10 flex items-center justify-center"><span class="iconify text-xl text-gold-400" data-icon="mdi:school"></span></div>
                        <div><span class="font-display font-bold text-base">BMCC</span><p class="text-[10px] text-bmcc-300 tracking-wider uppercase">Est. 1943 • Pune</p></div>
                    </div>
                    <p class="text-sm text-bmcc-300 leading-relaxed">Brihan Maharashtra College of Commerce, affiliated to Savitribai Phule Pune University. A premier commerce institution.</p>
                </div>
                <div>
                    <h4 class="font-display font-semibold text-sm text-bmcc-200 mb-4">Quick Links</h4>
                    <ul class="space-y-2.5">
                        <li><a href="https://www.bmcc.ac.in/" target="_blank" class="text-sm text-bmcc-400 hover:text-gold-400 transition-colors flex items-center gap-2"><span class="iconify" data-icon="mdi:open-in-new"></span>BMCC Official Website</a></li>
                        <li><button onclick="navigateTo('browse')" class="text-sm text-bmcc-400 hover:text-gold-400 transition-colors">Browse Lost & Found</button></li>
                        <li><button onclick="openModal('reportLost')" class="text-sm text-bmcc-400 hover:text-gold-400 transition-colors">Report Lost Item</button></li>
                        <li><button onclick="openModal('reportFound')" class="text-sm text-bmcc-400 hover:text-gold-400 transition-colors">Report Found Item</button></li>
                    </ul>
                </div>
                <div>
                    <h4 class="font-display font-semibold text-sm text-bmcc-200 mb-4">Contact</h4>
                    <ul class="space-y-2.5">
                        <li class="flex items-center gap-2 text-sm text-bmcc-400"><span class="iconify text-gold-500" data-icon="mdi:map-marker"></span>Shivajinagar, Pune 411005</li>
                        <li class="flex items-center gap-2 text-sm text-bmcc-400"><span class="iconify text-gold-500" data-icon="mdi:phone"></span>020-2553 1531</li>
                        <li class="flex items-center gap-2 text-sm text-bmcc-400"><span class="iconify text-gold-500" data-icon="mdi:email"></span>principal@bmcc.ac.in</li>
                    </ul>
                </div>
            </div>
            <div class="border-t border-white/10 mt-10 pt-8 flex flex-col sm:flex-row items-center justify-between gap-4">
                <p class="text-xs text-bmcc-500">© 2025 BMCC Lost & Found Portal. All rights reserved.</p>
                <p class="text-xs text-bmcc-600">Built for the BMCC community</p>
            </div>
        </div>
    </footer>

    <!-- ====== REPORT LOST MODAL ====== -->
    <div id="reportLostModal" class="fixed inset-0 z-[80] hidden">
        <div class="modal-overlay absolute inset-0" onclick="closeModal('reportLost')"></div>
        <div class="relative z-10 flex items-center justify-center min-h-screen p-4">
            <div class="bg-white rounded-3xl shadow-2xl w-full max-w-lg max-h-[90vh] overflow-y-auto animate-scaleIn">
                <div class="p-6 lg:p-8">
                    <div class="flex items-center justify-between mb-6">
                        <div class="flex items-center gap-3">
                            <div class="w-10 h-10 rounded-xl bg-red-50 flex items-center justify-center"><span class="iconify text-xl text-red-600" data-icon="mdi:magnify-close"></span></div>
                            <div><h2 class="font-display font-bold text-lg text-neutral-900">Report Lost Item</h2><p class="text-xs text-neutral-500">Fill in the details below</p></div>
                        </div>
                        <button onclick="closeModal('reportLost')" class="p-2 rounded-lg hover:bg-surface-100 transition-colors"><span class="iconify text-xl text-neutral-400" data-icon="mdi:close"></span></button>
                    </div>
                    <form id="lostForm" onsubmit="handleReportLost(event)" class="space-y-4">
                        <!-- Photo Upload -->
                        <div>
                            <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Photo <span class="text-neutral-400 font-normal">(optional)</span></label>
                            <div id="lostUploadZone" class="upload-zone rounded-xl p-4 text-center cursor-pointer relative" onclick="document.getElementById('lostPhotoInput').click()"
                                 ondragover="event.preventDefault(); this.classList.add('drag-over')" ondragleave="this.classList.remove('drag-over')" ondrop="handleDrop(event, 'lost')">
                                <input type="file" id="lostPhotoInput" accept="image/*" class="hidden" onchange="handlePhotoSelect(this, 'lost')">
                                <div id="lostUploadPlaceholder">
                                    <div class="w-10 h-10 rounded-xl bg-surface-100 flex items-center justify-center mx-auto mb-2"><span class="iconify text-xl text-neutral-400" data-icon="mdi:camera-plus"></span></div>
                                    <p class="text-xs text-neutral-500">Click or drag a photo here</p>
                                    <p class="text-[10px] text-neutral-400 mt-0.5">JPG, PNG up to 5 MB</p>
                                </div>
                                <div id="lostUploadPreview" class="hidden">
                                    <img id="lostPreviewImg" src="" alt="Preview" class="max-h-40 mx-auto rounded-lg object-contain">
                                    <button type="button" onclick="event.stopPropagation(); removePhoto('lost')" class="mt-2 text-xs font-medium text-red-600 hover:text-red-700 flex items-center gap-1 mx-auto">
                                        <span class="iconify" data-icon="mdi:trash-can-outline"></span> Remove Photo
                                    </button>
                                </div>
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Item Name *</label>
                            <input type="text" name="itemName" required placeholder="e.g., Blue Backpack, Samsung Phone" class="w-full px-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none transition-all">
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Category *</label>
                                <select name="category" required class="w-full px-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none bg-white cursor-pointer">
                                    <option value="">Select</option>
                                    <option value="electronics">Electronics</option>
                                    <option value="cards">ID/Cards</option>
                                    <option value="bags">Bags/Wallets</option>
                                    <option value="books">Books/Notes</option>
                                    <option value="keys">Keys</option>
                                    <option value="clothing">Clothing</option>
                                    <option value="other">Other</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Date Lost *</label>
                                <input type="date" name="date" required class="w-full px-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none transition-all">
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Location *</label>
                            <select name="location" required class="w-full px-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none bg-white cursor-pointer">
                                <option value="">Select Location</option>
                                <option value="Main Building">Main Building</option>
                                <option value="Library">Library</option>
                                <option value="Computer Lab">Computer Lab</option>
                                <option value="Canteen">Canteen</option>
                                <option value="Auditorium">Auditorium</option>
                                <option value="Gymkhana">Gymkhana</option>
                                <option value="Parking Area">Parking Area</option>
                                <option value="Ground Floor Corridor">Ground Floor Corridor</option>
                                <option value="Classroom">Classroom</option>
                                <option value="Washroom">Washroom</option>
                                <option value="Other">Other</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Description *</label>
                            <textarea name="description" required rows="3" placeholder="Color, brand, distinguishing features, etc." class="w-full px-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none transition-all resize-none"></textarea>
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Your Name *</label>
                            <input type="text" name="reporterName" required placeholder="Full Name" class="w-full px-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none transition-all">
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Class & Division</label>
                                <input type="text" name="class" placeholder="e.g., TY BCom A" class="w-full px-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none transition-all">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Phone Number *</label>
                                <input type="tel" name="phone" required placeholder="Mobile No." class="w-full px-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none transition-all">
                            </div>
                        </div>
                        <div class="flex gap-3 pt-2">
                            <button type="button" onclick="closeModal('reportLost')" class="flex-1 py-3 rounded-xl border border-surface-200 text-sm font-semibold text-neutral-600 hover:bg-surface-50 transition-all">Cancel</button>
                            <button type="submit" class="flex-1 btn-primary text-white py-3 rounded-xl text-sm font-semibold">Submit Report</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>

    <!-- ====== REPORT FOUND MODAL ====== -->
    <div id="reportFoundModal" class="fixed inset-0 z-[80] hidden">
        <div class="modal-overlay absolute inset-0" onclick="closeModal('reportFound')"></div>
        <div class="relative z-10 flex items-center justify-center min-h-screen p-4">
            <div class="bg-white rounded-3xl shadow-2xl w-full max-w-lg max-h-[90vh] overflow-y-auto animate-scaleIn">
                <div class="p-6 lg:p-8">
                    <div class="flex items-center justify-between mb-6">
                        <div class="flex items-center gap-3">
                            <div class="w-10 h-10 rounded-xl bg-green-50 flex items-center justify-center"><span class="iconify text-xl text-green-600" data-icon="mdi:hand-coin"></span></div>
                            <div><h2 class="font-display font-bold text-lg text-neutral-900">Report Found Item</h2><p class="text-xs text-neutral-500">Help someone get their item back</p></div>
                        </div>
                        <button onclick="closeModal('reportFound')" class="p-2 rounded-lg hover:bg-surface-100 transition-colors"><span class="iconify text-xl text-neutral-400" data-icon="mdi:close"></span></button>
                    </div>
                    <form id="foundForm" onsubmit="handleReportFound(event)" class="space-y-4">
                        <!-- Photo Upload -->
                        <div>
                            <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Photo <span class="text-neutral-400 font-normal">(optional)</span></label>
                            <div id="foundUploadZone" class="upload-zone rounded-xl p-4 text-center cursor-pointer relative" onclick="document.getElementById('foundPhotoInput').click()"
                                 ondragover="event.preventDefault(); this.classList.add('drag-over')" ondragleave="this.classList.remove('drag-over')" ondrop="handleDrop(event, 'found')">
                                <input type="file" id="foundPhotoInput" accept="image/*" class="hidden" onchange="handlePhotoSelect(this, 'found')">
                                <div id="foundUploadPlaceholder">
                                    <div class="w-10 h-10 rounded-xl bg-surface-100 flex items-center justify-center mx-auto mb-2"><span class="iconify text-xl text-neutral-400" data-icon="mdi:camera-plus"></span></div>
                                    <p class="text-xs text-neutral-500">Click or drag a photo here</p>
                                    <p class="text-[10px] text-neutral-400 mt-0.5">JPG, PNG up to 5 MB</p>
                                </div>
                                <div id="foundUploadPreview" class="hidden">
                                    <img id="foundPreviewImg" src="" alt="Preview" class="max-h-40 mx-auto rounded-lg object-contain">
                                    <button type="button" onclick="event.stopPropagation(); removePhoto('found')" class="mt-2 text-xs font-medium text-red-600 hover:text-red-700 flex items-center gap-1 mx-auto">
                                        <span class="iconify" data-icon="mdi:trash-can-outline"></span> Remove Photo
                                    </button>
                                </div>
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Item Name *</label>
                            <input type="text" name="itemName" required placeholder="e.g., Student ID Card, Black Wallet" class="w-full px-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none transition-all">
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Category *</label>
                                <select name="category" required class="w-full px-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none bg-white cursor-pointer">
                                    <option value="">Select</option>
                                    <option value="electronics">Electronics</option>
                                    <option value="cards">ID/Cards</option>
                                    <option value="bags">Bags/Wallets</option>
                                    <option value="books">Books/Notes</option>
                                    <option value="keys">Keys</option>
                                    <option value="clothing">Clothing</option>
                                    <option value="other">Other</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Date Found *</label>
                                <input type="date" name="date" required class="w-full px-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none transition-all">
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Location Found *</label>
                            <select name="location" required class="w-full px-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none bg-white cursor-pointer">
                                <option value="">Select Location</option>
                                <option value="Main Building">Main Building</option>
                                <option value="Library">Library</option>
                                <option value="Computer Lab">Computer Lab</option>
                                <option value="Canteen">Canteen</option>
                                <option value="Auditorium">Auditorium</option>
                                <option value="Gymkhana">Gymkhana</option>
                                <option value="Parking Area">Parking Area</option>
                                <option value="Ground Floor Corridor">Ground Floor Corridor</option>
                                <option value="Classroom">Classroom</option>
                                <option value="Washroom">Washroom</option>
                                <option value="Other">Other</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Description *</label>
                            <textarea name="description" required rows="3" placeholder="Color, brand, any visible name/number, etc." class="w-full px-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none transition-all resize-none"></textarea>
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Your Name *</label>
                            <input type="text" name="reporterName" required placeholder="Full Name" class="w-full px-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none transition-all">
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Class & Division</label>
                                <input type="text" name="class" placeholder="e.g., SY BCom C" class="w-full px-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none transition-all">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-neutral-700 mb-1.5">Phone Number *</label>
                                <input type="tel" name="phone" required placeholder="Mobile No." class="w-full px-4 py-2.5 rounded-xl border border-surface-200 text-sm input-focus outline-none transition-all">
                            </div>
                        </div>
                        <div class="flex gap-3 pt-2">
                            <button type="button" onclick="closeModal('reportFound')" class="flex-1 py-3 rounded-xl border border-surface-200 text-sm font-semibold text-neutral-600 hover:bg-surface-50 transition-all">Cancel</button>
                            <button type="submit" class="flex-1 btn-gold text-bmcc-950 py-3 rounded-xl text-sm font-semibold">Submit Report</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>

    <!-- ====== ITEM DETAIL MODAL ====== -->
    <div id="itemDetailModal" class="fixed inset-0 z-[80] hidden">
        <div class="modal-overlay absolute inset-0" onclick="closeModal('itemDetail')"></div>
        <div class="relative z-10 flex items-center justify-center min-h-screen p-4">
            <div id="itemDetailContent" class="bg-white rounded-3xl shadow-2xl w-full max-w-lg max-h-[90vh] overflow-y-auto animate-scaleIn"></div>
        </div>
    </div>

    <script>
        const categoryConfig = {
            electronics: { icon: 'mdi:cellphone', label: 'Electronics', color: 'text-blue-600', bg: 'bg-blue-50' },
            cards: { icon: 'mdi:card-account-details', label: 'ID/Cards', color: 'text-purple-600', bg: 'bg-purple-50' },
            bags: { icon: 'mdi:bag-personal', label: 'Bags/Wallets', color: 'text-amber-600', bg: 'bg-amber-50' },
            books: { icon: 'mdi:book-open-variant', label: 'Books/Notes', color: 'text-emerald-600', bg: 'bg-emerald-50' },
            keys: { icon: 'mdi:key', label: 'Keys', color: 'text-orange-600', bg: 'bg-orange-50' },
            clothing: { icon: 'mdi:tshirt-crew', label: 'Clothing', color: 'text-pink-600', bg: 'bg-pink-50' },
            other: { icon: 'mdi:help-circle', label: 'Other', color: 'text-neutral-600', bg: 'bg-neutral-50' }
        };

        let items = [];
        let currentFilter = { tab: 'all', category: 'all', search: '', sort: 'newest' };
        let photoData = { lost: null, found: null };

        // ============ PHOTO HANDLING ============
        function handlePhotoSelect(input, formType) {
            const file = input.files[0];
            if (!file) return;
            processFile(file, formType);
        }

        function handleDrop(e, formType) {
            e.preventDefault();
            e.currentTarget.classList.remove('drag-over');
            const file = e.dataTransfer.files[0];
            if (file && file.type.startsWith('image/')) {
                processFile(file, formType);
            } else {
                showToast('Please drop a valid image file (JPG, PNG)', 'error');
            }
        }

        function processFile(file, formType) {
            if (file.size > 5 * 1024 * 1024) {
                showToast('Image must be under 5 MB', 'error');
                return;
            }
            if (!file.type.startsWith('image/')) {
                showToast('Only image files are allowed', 'error');
                return;
            }
            const reader = new FileReader();
            reader.onload = function(e) {
                photoData[formType] = e.target.result;
                const zone = document.getElementById(formType + 'UploadZone');
                const placeholder = document.getElementById(formType + 'UploadPlaceholder');
                const preview = document.getElementById(formType + 'UploadPreview');
                const img = document.getElementById(formType + 'PreviewImg');
                img.src = e.target.result;
                placeholder.classList.add('hidden');
                preview.classList.remove('hidden');
                zone.classList.add('has-image');
            };
            reader.readAsDataURL(file);
        }

        function removePhoto(formType) {
            photoData[formType] = null;
            const zone = document.getElementById(formType + 'UploadZone');
            const placeholder = document.getElementById(formType + 'UploadPlaceholder');
            const preview = document.getElementById(formType + 'UploadPreview');
            const input = document.getElementById(formType + 'PhotoInput');
            placeholder.classList.remove('hidden');
            preview.classList.add('hidden');
            zone.classList.remove('has-image');
            input.value = '';
        }

        // ============ RENDERING ============
        function getFilteredItems() {
            let filtered = [...items];
            if (currentFilter.tab === 'lost') filtered = filtered.filter(i => i.type === 'lost' && i.status === 'active');
            else if (currentFilter.tab === 'found') filtered = filtered.filter(i => i.type === 'found' && i.status === 'active');
            else if (currentFilter.tab === 'claimed') filtered = filtered.filter(i => i.status === 'claimed');
            if (currentFilter.category !== 'all') filtered = filtered.filter(i => i.category === currentFilter.category);
            if (currentFilter.search) {
                const q = currentFilter.search.toLowerCase();
                filtered = filtered.filter(i => i.itemName.toLowerCase().includes(q) || i.location.toLowerCase().includes(q) || i.description.toLowerCase().includes(q) || i.category.toLowerCase().includes(q));
            }
            if (currentFilter.sort === 'oldest') filtered.reverse();
            else if (currentFilter.sort === 'category') filtered.sort((a, b) => a.category.localeCompare(b.category));
            return filtered;
        }

        function renderItems() {
            const grid = document.getElementById('itemsGrid');
            const empty = document.getElementById('emptyState');
            const loadMore = document.getElementById('loadMoreContainer');
            const filtered = getFilteredItems();

            if (filtered.length === 0) {
                grid.innerHTML = '';
                empty.classList.remove('hidden');
                loadMore.classList.add('hidden');
                return;
            }
            empty.classList.add('hidden');
            loadMore.classList.remove('hidden');

            grid.innerHTML = filtered.map((item, index) => {
                const cat = categoryConfig[item.category];
                const statusBadge = item.status === 'claimed'
                    ? '<span class="badge-claimed text-[10px] font-semibold px-2.5 py-1 rounded-full">Claimed</span>'
                    : item.type === 'loss'
                        ? '<span class="badge-lost text-[10px] font-semibold px-2.5 py-1 rounded-full">Lost</span>'
                        : '<span class="badge-found text-[10px] font-semibold px-2.5 py-1 rounded-full">Found</span>';

                const imageSection = item.photo
                    ? `<div class="w-full h-36 rounded-xl overflow-hidden bg-surface-100 mb-3">
                           <img src="${item.photo}" alt="${item.itemName}" class="w-full h-full object-cover hover:scale-105 transition-transform duration-500">
                       </div>`
                    : `<div class="w-11 h-11 rounded-xl ${cat.bg} flex items-center justify-center flex-shrink-0">
                           <span class="iconify text-xl ${cat.color}" data-icon="${cat.icon}"></span>
                       </div>`;

                const layoutClass = item.photo ? '' : 'flex items-start gap-3';

                return `
                <div class="bg-white rounded-2xl border border-surface-200 overflow-hidden card-hover cursor-pointer animate-fadeInUp stagger-${Math.min(index + 1, 8)} ${item.status === 'claimed' ? 'opacity-60' : ''}"
                     onclick="openItemDetail(${item.id})">
                    <div class="p-5">
                        <div class="flex items-center justify-between mb-3">
                            ${statusBadge}
                            <span class="text-[10px] text-neutral-400">${item.timeAgo}</span>
                        </div>
                        ${item.photo ? imageSection : `<div class="${layoutClass}">${imageSection}<div class="min-w-0"><h3 class="font-display font-bold text-sm text-neutral-900 truncate">${item.itemName}</h3><p class="text-xs text-neutral-500 mt-0.5 line-clamp-1">${item.description}</p></div></div>`}
                        <div class="flex items-center gap-2 mt-3 pt-3 border-t border-surface-100">
                            <span class="iconify text-xs text-neutral-400" data-icon="mdi:map-marker"></span>
                            <span class="text-xs text-neutral-500">${item.location}</span>
                        </div>
                    </div>
                </div>`;
            }).join('');
        }

        function openItemDetail(id) {
            const item = items.find(i => i.id === id);
            if (!item) return;
            const cat = categoryConfig[item.category];
            const statusBadge = item.status === 'claimed'
                ? '<span class="badge-claimed text-xs font-semibold px-3 py-1.5 rounded-full">Claimed</span>'
                : item.type === 'loss'
                    ? '<span class="badge-lost text-xs font-semibold px-3 py-1.5 rounded-full">Lost</span>'
                    : '<span class="badge-found text-xs font-semibold px-3 py-1.5 rounded-full">Found</span>';

            const photoSection = item.photo
                ? `<div class="w-full rounded-xl overflow-hidden bg-surface-100 mb-4"><img src="${item.photo}" alt="${item.itemName}" class="w-full max-h-64 object-contain"></div>`
                : '';

            let claimSection = '';
            if (item.status === 'active') {
                if (item.type === 'found') {
                    claimSection = `<div class="mt-6 pt-5 border-t border-surface-100">
                        <p class="text-xs text-neutral-500 mb-3">Think this is yours? Contact the finder or claim at the office.</p>
                        <div class="flex gap-3">
                            <button onclick="claimItem(${item.id})" class="flex-1 btn-primary text-white py-2.5 rounded-xl text-sm font-semibold flex items-center justify-center gap-2"><span class="iconify" data-icon="mdi:hand-back-right"></span> Claim This Item</button>
                            <a href="tel:${item.phone}" class="px-4 py-2.5 rounded-xl border border-surface-200 text-sm font-semibold text-neutral-600 hover:bg-surface-50 transition-all flex items-center gap-2"><span class="iconify" data-icon="mdi:phone"></span></a>
                        </div></div>`;
                } else {
                    claimSection = `<div class="mt-6 pt-5 border-t border-surface-100">
                        <p class="text-xs text-neutral-500 mb-3">Have you found this item? Help the owner recover it.</p>
                        <a href="tel:${item.phone}" class="flex-1 btn-gold text-bmcc-950 py-2.5 rounded-xl text-sm font-semibold flex items-center justify-center gap-2"><span class="iconify" data-icon="mdi:phone"></span> Contact Owner</a>
                    </div>`;
                }
            } else {
                claimSection = `<div class="mt-6 pt-5 border-t border-surface-100"><div class="flex items-center gap-2 text-sm text-green-700 font-medium"><span class="iconify" data-icon="mdi:check-circle"></span>${item.claimedBy}</div></div>`;
            }

            document.getElementById('itemDetailContent').innerHTML = `
                <div class="p-6 lg:p-8">
                    <div class="flex items-center justify-between mb-5">
                        <div class="flex items-center gap-3">
                            <div class="w-12 h-12 rounded-xl ${cat.bg} flex items-center justify-center"><span class="iconify text-2xl ${cat.color}" data-icon="${cat.icon}"></span></div>
                            <div><div class="flex items-center gap-2">${statusBadge}</div><h2 class="font-display font-bold text-lg text-neutral-900 mt-0.5">${item.itemName}</h2></div>
                        </div>
                        <button onclick="closeModal('itemDetail')" class="p-2 rounded-lg hover:bg-surface-100 transition-colors"><span class="iconify text-xl text-neutral-400" data-icon="mdi:close"></span></button>
                    </div>
                    ${photoSection}
                    <div class="space-y-3">
                        <div class="flex items-start gap-3 p-3 rounded-xl bg-surface-50">
                            <span class="iconify text-lg text-neutral-400 mt-0.5" data-icon="mdi:text-box"></span>
                            <div><p class="text-xs font-semibold text-neutral-700 mb-0.5">Description</p><p class="text-sm text-neutral-600 leading-relaxed">${item.description}</p></div>
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <div class="flex items-center gap-3 p-3 rounded-xl bg-surface-50"><span class="iconify text-lg text-neutral-400" data-icon="mdi:map-marker"></span><div><p class="text-xs font-semibold text-neutral-700">Location</p><p class="text-sm text-neutral-600">${item.location}</p></div></div>
                            <div class="flex items-center gap-3 p-3 rounded-xl bg-surface-50"><span class="iconify text-lg text-neutral-400" data-icon="mdi:calendar"></span><div><p class="text-xs font-semibold text-neutral-700">Date</p><p class="text-sm text-neutral-600">${formatDate(item.date)}</p></div></div>
                            <div class="flex items-center gap-3 p-3 rounded-xl bg-surface-50"><span class="iconify text-lg text-neutral-400" data-icon="mdi:account"></span><div><p class="text-xs font-semibold text-neutral-700">Reported By</p><p class="text-sm text-neutral-600">${item.reporterName}</p></div></div>
                            <div class="flex items-center gap-3 p-3 rounded-xl bg-surface-50"><span class="iconify text-lg text-neutral-400" data-icon="mdi:school"></span><div><p class="text-xs font-semibold text-neutral-700">Class</p><p class="text-sm text-neutral-600">${item.classInfo || 'N/A'}</p></div></div>
                        </div>
                    </div>
                    ${claimSection}
                </div>`;
            openModal('itemDetail');
        }

        // ============ FILTERS ============
        function setTab(tab) {
            currentFilter.tab = tab;
            document.querySelectorAll('.tab-btn').forEach(btn => { btn.classList.remove('active'); if (btn.dataset.tab === tab) btn.classList.add('active'); });
            renderItems();
        }
        function setCategory(cat) {
            currentFilter.category = cat;
            document.querySelectorAll('.category-chip').forEach(btn => { btn.classList.remove('active'); if (btn.dataset.cat === cat) btn.classList.add('active'); });
            renderItems();
        }
        function handleSearch(query) {
            currentFilter.search = query;
            document.getElementById('heroSearch').value = query;
            document.getElementById('browseSearch').value = query;
            renderItems();
        }
        function handleSort(value) { currentFilter.sort = value; renderItems(); }

        // ============ MODALS ============
        function openModal(name) { const m = document.getElementById(name + 'Modal'); if (m) { m.classList.remove('hidden'); document.body.style.overflow = 'hidden'; } }
        function closeModal(name) { const m = document.getElementById(name + 'Modal'); if (m) { m.classList.add('hidden'); document.body.style.overflow = ''; } }

        // ============ FORM HANDLERS ============
        function handleReportLost(e) {
            e.preventDefault();
            const form = e.target;
            items.unshift({
                id: Date.now(), type: 'loss', itemName: form.itemName.value, category: form.category.value,
                location: form.location.value, description: form.description.value, date: form.date.value,
                reporterName: form.reporterName.value, classInfo: form.class.value, phone: form.phone.value,
                status: 'active', timeAgo: 'Just now', photo: photoData.lost
            });
            form.reset();
            removePhoto('lost');
            setDefaultDates('lost');
            closeModal('reportLost');
            renderItems(); updateStats();
            showToast('Lost item reported successfully! We\'ll notify you if there\'s a match.', 'success');
        }

        function handleReportFound(e) {
            e.preventDefault();
            const form = e.target;
            items.unshift({
                id: Date.now(), type: 'found', itemName: form.itemName.value, category: form.category.value,
                location: form.location.value, description: form.description.value, date: form.date.value,
                reporterName: form.reporterName.value, classInfo: form.class.value, phone: form.phone.value,
                status: 'active', timeAgo: 'Just now', photo: photoData.found
            });
            form.reset();
            removePhoto('found');
            setDefaultDates('found');
            closeModal('reportFound');
            renderItems(); updateStats();
            showToast('Found item reported! Thank you for helping a fellow student.', 'success');
        }

        function claimItem(id) {
            const item = items.find(i => i.id === id);
            if (item) {
                item.status = 'claimed';
                item.claimedBy = 'Claimed — pending verification at office';
                closeModal('itemDetail'); renderItems(); updateStats();
                showToast('Claim submitted! Please visit the Student Welfare Office with your BMCC ID for verification.', 'success');
            }
        }

        // ============ NAV ============
        function navigateTo(section) {
            const map = { home: 'heroSection', browse: 'browseSection', howItWorks: 'howItWorksSection' };
            const el = document.getElementById(map[section]);
            if (el) window.scrollTo({ top: el.getBoundingClientRect().top + window.pageYOffset - 80, behavior: 'smooth' });
        }
        function toggleMobileMenu() {
            const menu = document.getElementById('mobileMenu'), icon = document.getElementById('menuIcon');
            const isOpen = !menu.classList.contains('hidden');
            menu.classList.toggle('hidden');
            icon.dataset.icon = isOpen ? 'mdi:menu' : 'mdi:close';
        }

        // ============ UTILS ============
        function formatDate(d) { return new Date(d).toLocaleDateString('en-IN', { day: 'numeric', month: 'short', year: 'numeric' }); }
        function setDefaultDates(prefix) { const t = new Date().toISOString().split('T')[0]; document.querySelectorAll('#' + prefix + 'Form input[type="date"]').forEach(i => i.value = t); }

        function updateStats() {
            const lost = items.filter(i => i.type === 'loss' && i.status === 'active').length;
            const found = items.filter(i => i.type === 'found' && i.status === 'active').length;
            const returned = items.filter(i => i.status === 'claimed').length;
            const total = items.length;
            document.getElementById('statLost').textContent = lost;
            document.getElementById('statFound').textContent = found;
            document.getElementById('statReturned').textContent = returned;
            document.getElementById('statAvgTime').textContent = returned > 0 ? '2.3d' : '—';
            document.getElementById('heroStatItems').textContent = total;
            document.getElementById('heroStatReturned').textContent = returned;
            document.getElementById('heroStatRate').textContent = total > 0 ? Math.round((returned / total) * 100) + '%' : '—';
        }

        function showToast(message, type = 'info') {
            const container = document.getElementById('toastContainer');
            const toast = document.createElement('div');
            const colors = { success: 'bg-green-600', error: 'bg-red-600', info: 'bg-bmcc-800' };
            const icons = { success: 'mdi:check-circle', error: 'mdi:alert-circle', info: 'mdi:information' };
            toast.className = `toast ${colors[type]} text-white px-5 py-3.5 rounded-2xl shadow-2xl flex items-start gap-3 max-w-sm animate-slideInRight`;
            toast.innerHTML = `<span class="iconify text-lg flex-shrink-0 mt-0.5" data-icon="${icons[type]}"></span><p class="text-sm leading-relaxed">${message}</p>`;
            container.appendChild(toast);
            setTimeout(() => { toast.style.transition = 'all 0.3s ease'; toast.style.opacity = '0'; toast.style.transform = 'translateX(20px)'; setTimeout(() => toast.remove(), 300); }, 4500);
        }

        document.addEventListener('DOMContentLoaded', () => {
            const today = new Date().toISOString().split('T')[0];
            document.querySelectorAll('input[type="date"]').forEach(i => i.value = today);
            renderItems(); updateStats();
        });
        document.addEventListener('keydown', (e) => { if (e.key === 'Escape') ['reportLost', 'reportFound', 'itemDetail'].forEach(n => closeModal(n)); });
    </script>
</body>
</html>
