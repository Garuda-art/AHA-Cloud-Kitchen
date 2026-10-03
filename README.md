# AHA-Cloud-Kitchen
<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>AHA Cloud Kitchen | Next-Gen 3D Culinary Experience</title>

  <!-- Google Fonts & Tailwind CSS -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&family=Playfair+Display:ital,wght@0,600;0,800;1,700&display=swap" rel="stylesheet">
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Three.js and OrbitControls -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>

  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            brand: {
              charcoal: '#0B0F17',
              dark: '#121824',
              card: '#161F30',
              border: '#233047',
              saffron: '#FF5400',
              gold: '#FFAE03',
              glow: '#FF7700',
              emerald: '#10B981',
              crimson: '#EF4444'
            }
          },
          fontFamily: {
            sans: ['"Plus Jakarta Sans"', 'sans-serif'],
            serif: ['"Playfair Display"', 'serif']
          },
          boxShadow: {
            'glow-saffron': '0 0 35px -5px rgba(255, 84, 0, 0.4)',
            'glow-gold': '0 0 30px -5px rgba(255, 174, 3, 0.3)',
            'inner-glow': 'inset 0 1px 1px 0 rgba(255, 255, 255, 0.1)'
          }
        }
      }
    };
  </script>

  <style>
    /* Custom Scrollbar */
    ::-webkit-scrollbar {
      width: 7px;
    }
    ::-webkit-scrollbar-track {
      background: #0B0F17;
    }
    ::-webkit-scrollbar-thumb {
      background: #233047;
      border-radius: 9999px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: #FF5400;
    }
    
    .glass-nav {
      background: rgba(11, 15, 23, 0.85);
      backdrop-filter: blur(14px);
      -webkit-backdrop-filter: blur(14px);
    }
    .glass-card {
      background: rgba(22, 31, 48, 0.7);
      backdrop-filter: blur(12px);
      border: 1px solid rgba(255, 255, 255, 0.07);
    }
    .text-gradient {
      background: linear-gradient(135deg, #FFF 20%, #FFAE03 60%, #FF5400 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }
    .badge-glow {
      box-shadow: 0 0 12px rgba(255, 84, 0, 0.35);
    }
  </style>
</head>
<body class="bg-brand-charcoal text-slate-100 font-sans antialiased overflow-x-hidden selection:bg-brand-saffron selection:text-white">

  <!-- Top Banner -->
  <header class="fixed top-0 left-0 right-0 z-50 glass-nav border-b border-brand-border/60 transition-all duration-300" id="mainHeader">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex items-center justify-between h-20">
        
        <!-- Logo -->
        <a href="#hero" class="flex items-center space-x-3 group">
          <div class="w-11 h-11 rounded-2xl bg-gradient-to-tr from-brand-saffron via-brand-gold to-orange-400 flex items-center justify-center shadow-glow-saffron transform group-hover:scale-105 transition-transform duration-300">
            <i data-lucide="flame" class="w-6 h-6 text-brand-charcoal fill-current"></i>
          </div>
          <div>
            <span class="text-2xl font-extrabold tracking-tight font-serif text-white">AHA</span>
            <span class="text-xs uppercase tracking-widest text-brand-gold block font-semibold -mt-1">Cloud Kitchen</span>
          </div>
        </a>

        <!-- Desktop Navigation -->
        <nav class="hidden md:flex items-center space-x-8 text-sm font-medium">
          <a href="#hero" class="text-slate-300 hover:text-brand-gold transition-colors">3D Showcase</a>
          <a href="#brands" class="text-slate-300 hover:text-brand-gold transition-colors">Our Brands</a>
          <a href="#menu" class="text-slate-300 hover:text-brand-gold transition-colors">Digital Menu</a>
          <a href="#metrics" class="text-slate-300 hover:text-brand-gold transition-colors">Live Kitchen Ops</a>
          <a href="#tracking" class="text-slate-300 hover:text-brand-gold transition-colors">Live Tracker</a>
        </nav>

        <!-- Right Side CTA and Cart Button -->
        <div class="flex items-center space-x-4">
          <!-- Audio Toggle -->
          <button id="soundToggleBtn" onclick="toggleSound()" title="Toggle Sound FX" class="p-2.5 rounded-xl border border-brand-border text-slate-400 hover:text-white hover:border-brand-saffron bg-brand-dark/80 transition-all">
            <i data-lucide="volume-2" id="soundIcon" class="w-5 h-5"></i>
          </button>

          <!-- Cart Trigger -->
          <button onclick="toggleCartDrawer(true)" class="relative flex items-center space-x-2 bg-gradient-to-r from-brand-saffron to-brand-gold hover:opacity-95 text-brand-charcoal font-bold px-4 py-2.5 rounded-xl shadow-glow-saffron transition-all duration-300">
            <i data-lucide="shopping-bag" class="w-5 h-5"></i>
            <span class="text-sm font-bold">Cart</span>
            <span id="cartCountBadge" class="bg-brand-charcoal text-white text-xs px-2 py-0.5 rounded-full font-bold">0</span>
          </button>

          <!-- Mobile Menu Hamburger -->
          <button id="mobileNavToggle" onclick="toggleMobileNav()" class="md:hidden p-2 rounded-xl text-slate-300 hover:text-white border border-brand-border">
            <i data-lucide="menu" class="w-6 h-6"></i>
          </button>
        </div>
      </div>
    </div>

    <!-- Mobile Drawer Menu -->
    <div id="mobileMenu" class="hidden md:hidden bg-brand-dark/95 border-b border-brand-border px-6 py-4 space-y-3">
      <a href="#hero" onclick="toggleMobileNav()" class="block text-slate-300 hover:text-brand-gold py-1">3D Showcase</a>
      <a href="#brands" onclick="toggleMobileNav()" class="block text-slate-300 hover:text-brand-gold py-1">Our Brands</a>
      <a href="#menu" onclick="toggleMobileNav()" class="block text-slate-300 hover:text-brand-gold py-1">Digital Menu</a>
      <a href="#metrics" onclick="toggleMobileNav()" class="block text-slate-300 hover:text-brand-gold py-1">Live Ops & Hygiene</a>
      <a href="#tracking" onclick="toggleMobileNav()" class="block text-slate-300 hover:text-brand-gold py-1">Order Tracker</a>
    </div>
  </header>

  <!-- HERO SECTION WITH 3D CANVAS -->
  <section id="hero" class="relative min-h-screen pt-28 pb-16 flex items-center overflow-hidden">
    <!-- Ambient Background Gradients -->
    <div class="absolute top-1/4 -left-32 w-96 h-96 bg-brand-saffron/15 rounded-full blur-3xl pointer-events-none"></div>
    <div class="absolute bottom-10 right-0 w-[500px] h-[500px] bg-brand-gold/10 rounded-full blur-[120px] pointer-events-none"></div>

    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 w-full relative z-10">
      <div class="grid grid-cols-1 lg:grid-cols-12 gap-8 items-center min-h-[calc(100vh-140px)]">
        
        <!-- Left Hero Copy -->
        <div class="lg:col-span-5 space-y-6 pt-4 lg:pt-0">
          <div class="inline-flex items-center space-x-2.5 px-3.5 py-1.5 rounded-full bg-brand-dark/80 border border-brand-border text-xs font-semibold text-brand-gold badge-glow">
            <span class="w-2 h-2 rounded-full bg-emerald-400 animate-ping"></span>
            <span>HYPER-CONNECTED 4-IN-1 CLOUD KITCHEN</span>
          </div>

          <h1 class="text-4xl sm:text-5xl lg:text-6xl font-extrabold tracking-tight leading-[1.15]">
            Culinary Magic <br />
            <span class="text-gradient font-serif italic">Delivered To You.</span>
          </h1>

          <p class="text-slate-300 text-base sm:text-lg leading-relaxed font-normal">
            Experience multi-brand virtual dining engineered with Michelin standard precision. Sterile, contactless packaging, 12-minute prep times, and flavors engineered to perfection.
          </p>

          <!-- Interactive 3D Dish Selector Tabs -->
          <div class="p-3 bg-brand-dark/70 rounded-2xl border border-brand-border space-y-2">
            <div class="text-xs uppercase tracking-wider text-slate-400 font-semibold px-2">Inspect 3D Signature Creations:</div>
            <div class="grid grid-cols-3 gap-2">
              <button onclick="switchDishModel('burger')" id="dishTab-burger" class="dish-selector-btn active-dish-tab px-3 py-2 text-xs font-bold rounded-xl transition-all duration-200 bg-brand-saffron text-white flex items-center justify-center space-x-1.5">
                <span>🍔 Smoke Forge</span>
              </button>
              <button onclick="switchDishModel('biryani')" id="dishTab-biryani" class="dish-selector-btn px-3 py-2 text-xs font-bold rounded-xl transition-all duration-200 bg-brand-card hover:bg-brand-border text-slate-300 flex items-center justify-center space-x-1.5">
                <span>🍲 Dum Royal</span>
              </button>
              <button onclick="switchDishModel('cloche')" id="dishTab-cloche" class="dish-selector-btn px-3 py-2 text-xs font-bold rounded-xl transition-all duration-200 bg-brand-card hover:bg-brand-border text-slate-300 flex items-center justify-center space-x-1.5">
                <span>✨ Cloche Glow</span>
              </button>
            </div>
          </div>

          <!-- Quick Action Buttons -->
          <div class="flex flex-wrap items-center gap-4 pt-2">
            <a href="#menu" class="px-7 py-3.5 rounded-xl font-bold bg-gradient-to-r from-brand-saffron to-brand-gold text-brand-charcoal hover:scale-[1.02] shadow-glow-saffron transition-all duration-200 inline-flex items-center space-x-2">
              <span>Order Now</span>
              <i data-lucide="arrow-right" class="w-4 h-4"></i>
            </a>
            <button onclick="reset3DCamera()" class="px-5 py-3.5 rounded-xl font-semibold bg-brand-card hover:bg-brand-border text-slate-200 border border-brand-border/80 transition-all inline-flex items-center space-x-2">
              <i data-lucide="rotate-ccw" class="w-4 h-4"></i>
              <span>Reset 3D View</span>
            </button>
          </div>

          <!-- Value props chips -->
          <div class="grid grid-cols-3 gap-3 pt-4 border-t border-brand-border/60">
            <div>
              <p class="text-xl font-black text-white font-mono">12 <span class="text-xs font-sans text-brand-gold">MIN</span></p>
              <p class="text-xs text-slate-400">Avg Dispatch</p>
            </div>
            <div>
              <p class="text-xl font-black text-white font-mono">100%</p>
              <p class="text-xs text-slate-400">Sterile Seal</p>
            </div>
            <div>
              <p class="text-xl font-black text-white font-mono">4.9 ★</p>
              <p class="text-xs text-slate-400">Google Verified</p>
            </div>
          </div>
        </div>

        <!-- Right 3D Interactive Canvas Canvas Container -->
        <div class="lg:col-span-7 h-[420px] sm:h-[520px] lg:h-[620px] relative w-full rounded-3xl glass-card overflow-hidden border border-brand-border shadow-2xl">
          <!-- 3D Canvas element -->
          <div id="threeContainer" class="w-full h-full cursor-grab active:cursor-grabbing"></div>

          <!-- 3D Viewport Badges & Hint -->
          <div class="absolute top-4 left-4 bg-brand-charcoal/80 border border-brand-border px-3 py-1.5 rounded-xl text-xs backdrop-blur-md flex items-center space-x-2">
            <span class="w-2 h-2 rounded-full bg-brand-saffron animate-pulse"></span>
            <span id="current3DLabel" class="font-medium text-slate-200">Smash Burger Loaded Deluxe</span>
          </div>

          <div class="absolute bottom-4 right-4 bg-brand-charcoal/80 border border-brand-border px-3 py-1.5 rounded-xl text-xs backdrop-blur-md text-slate-300 flex items-center space-x-2 pointer-events-none">
            <i data-lucide="move" class="w-3.5 h-3.5 text-brand-gold"></i>
            <span>Drag to rotate • Scroll to zoom</span>
          </div>

          <!-- Interactive Hot-Spot Infoboxes -->
          <div id="hotspotBox" class="absolute top-4 right-4 max-w-xs bg-brand-dark/95 border border-brand-gold/40 p-3.5 rounded-2xl backdrop-blur-md shadow-glow-gold transition-all duration-300 opacity-90 hover:opacity-100">
            <div class="flex items-center justify-between text-xs font-bold text-brand-gold uppercase tracking-wider mb-1">
              <span id="hotspotCategory">Ingredient Spotlight</span>
              <span class="text-[10px] bg-brand-gold/20 text-brand-gold px-1.5 py-0.5 rounded">3D Live</span>
            </div>
            <h4 id="hotspotTitle" class="text-sm font-bold text-white mb-1">Black Angus Beef Patty</h4>
            <p id="hotspotDesc" class="text-xs text-slate-300 leading-snug">Seared at 450°F to lock in natural juices, topped with caramelized onion reductions and cheddar.</p>
          </div>

          <!-- Explode Ingredients Button -->
          <div class="absolute bottom-4 left-4">
            <button id="explodeBtn" onclick="toggleExplodedView()" class="px-3.5 py-2 rounded-xl text-xs font-bold bg-brand-card hover:bg-brand-dark border border-brand-border text-slate-200 flex items-center space-x-2 transition-all">
              <i data-lucide="layers" class="w-3.5 h-3.5 text-brand-saffron"></i>
              <span id="explodeBtnText">Deconstruct Layers</span>
            </button>
          </div>
        </div>

      </div>
    </div>
  </section>

  <!-- MULTI-BRAND SHOWCASE -->
  <section id="brands" class="py-20 bg-brand-dark/50 border-y border-brand-border/60 relative">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="text-center max-w-3xl mx-auto mb-14">
        <span class="text-brand-saffron text-xs font-bold tracking-widest uppercase">The AHA Virtual Food Hall</span>
        <h2 class="text-3xl sm:text-4xl font-extrabold text-white mt-2">4 Signature Brands, 1 High-Tech Kitchen</h2>
        <p class="text-slate-400 mt-3 text-sm sm:text-base">Multiple cravings in one household? Order seamlessly across all our specialized culinary houses with a single checkout and zero extra delivery surcharges.</p>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
        
        <!-- Brand 1 -->
        <div class="glass-card rounded-2xl p-6 hover:border-brand-saffron/50 transition-all duration-300 group hover:-translate-y-1">
          <div class="w-14 h-14 rounded-2xl bg-amber-500/10 text-brand-gold flex items-center justify-center text-3xl mb-4 group-hover:scale-110 transition-transform">
            🍗
          </div>
          <span class="text-[11px] font-bold uppercase tracking-wider text-brand-gold">Heritage Flavors</span>
          <h3 class="text-xl font-bold text-white mt-1">AHA Biryani Express</h3>
          <p class="text-slate-400 text-xs sm:text-sm mt-2 leading-relaxed">
            Slow-cooked royal dum biryanis, aromatic saffron basmati, and 24-spice Hyderabadi secret marinades.
          </p>
          <div class="mt-4 pt-4 border-t border-brand-border flex items-center justify-between text-xs text-slate-300">
            <span>🔥 4 Dum Kettles Active</span>
            <button onclick="filterMenuByBrand('biryani')" class="text-brand-gold font-bold hover:underline">Explore</button>
          </div>
        </div>

        <!-- Brand 2 -->
        <div class="glass-card rounded-2xl p-6 hover:border-brand-saffron/50 transition-all duration-300 group hover:-translate-y-1">
          <div class="w-14 h-14 rounded-2xl bg-orange-500/10 text-brand-saffron flex items-center justify-center text-3xl mb-4 group-hover:scale-110 transition-transform">
            🍔
          </div>
          <span class="text-[11px] font-bold uppercase tracking-wider text-brand-saffron">Artisanal Fast-Casual</span>
          <h3 class="text-xl font-bold text-white mt-1">Burger Forge 3D</h3>
          <p class="text-slate-400 text-xs sm:text-sm mt-2 leading-relaxed">
            Double smashed crust patties, toasted brioche, house-made truffle aioli and crispy golden shoestring fries.
          </p>
          <div class="mt-4 pt-4 border-t border-brand-border flex items-center justify-between text-xs text-slate-300">
            <span>⚡ 6 Mins Prep</span>
            <button onclick="filterMenuByBrand('burger')" class="text-brand-saffron font-bold hover:underline">Explore</button>
          </div>
        </div>

        <!-- Brand 3 -->
        <div class="glass-card rounded-2xl p-6 hover:border-brand-saffron/50 transition-all duration-300 group hover:-translate-y-1">
          <div class="w-14 h-14 rounded-2xl bg-red-500/10 text-red-400 flex items-center justify-center text-3xl mb-4 group-hover:scale-110 transition-transform">
            🥡
          </div>
          <span class="text-[11px] font-bold uppercase tracking-wider text-red-400">Wok & Dim Sum</span>
          <h3 class="text-xl font-bold text-white mt-1">AHA Wok Republic</h3>
          <p class="text-slate-400 text-xs sm:text-sm mt-2 leading-relaxed">
            Fiery Szechuan wok-tossed noodles, crystalline steamed dim sums, and fiery chili-garlic glazed bowls.
          </p>
          <div class="mt-4 pt-4 border-t border-brand-border flex items-center justify-between text-xs text-slate-300">
            <span>🥢 High-heat Wok Station</span>
            <button onclick="filterMenuByBrand('asian')" class="text-red-400 font-bold hover:underline">Explore</button>
          </div>
        </div>

        <!-- Brand 4 -->
        <div class="glass-card rounded-2xl p-6 hover:border-brand-saffron/50 transition-all duration-300 group hover:-translate-y-1">
          <div class="w-14 h-14 rounded-2xl bg-emerald-500/10 text-emerald-400 flex items-center justify-center text-3xl mb-4 group-hover:scale-110 transition-transform">
            🥗
          </div>
          <span class="text-[11px] font-bold uppercase tracking-wider text-emerald-400">Clean & Vibrant</span>
          <h3 class="text-xl font-bold text-white mt-1">Green Oasis</h3>
          <p class="text-slate-400 text-xs sm:text-sm mt-2 leading-relaxed">
            Superfood quinoa bowls, cold-pressed raw tonics, keto wraps, and wholesome calorie-conscious delights.
          </p>
          <div class="mt-4 pt-4 border-t border-brand-border flex items-center justify-between text-xs text-slate-300">
            <span>🌱 Farm-Fresh Daily</span>
            <button onclick="filterMenuByBrand('healthy')" class="text-emerald-400 font-bold hover:underline">Explore</button>
          </div>
        </div>

      </div>
    </div>
  </section>

  <!-- DIGITAL MENU SECTION -->
  <section id="menu" class="py-20 relative">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      
      <div class="flex flex-col md:flex-row md:items-end justify-between mb-10 gap-6">
        <div>
          <span class="text-brand-saffron text-xs font-bold tracking-widest uppercase">Live Kitchen Dispatch</span>
          <h2 class="text-3xl sm:text-4xl font-extrabold text-white mt-1">Hand-Crafted Menu</h2>
          <p class="text-slate-400 text-sm mt-1">Prepared on-demand using sterile automated thermal stations.</p>
        </div>

        <!-- Search & Filter Controls -->
        <div class="flex flex-wrap items-center gap-2">
          <div class="relative">
            <input type="text" id="menuSearchInput" onkeyup="searchMenuItems()" placeholder="Search dishes, ingredients..." class="bg-brand-card border border-brand-border rounded-xl px-4 py-2 text-xs text-white placeholder-slate-400 focus:outline-none focus:border-brand-saffron w-56" />
            <i data-lucide="search" class="w-3.5 h-3.5 absolute right-3 top-3 text-slate-400 pointer-events-none"></i>
          </div>

          <button onclick="filterDiet('all')" id="diet-all" class="diet-btn px-3 py-2 rounded-xl text-xs font-semibold bg-brand-saffron text-white">All</button>
          <button onclick="filterDiet('veg')" id="diet-veg" class="diet-btn px-3 py-2 rounded-xl text-xs font-semibold bg-brand-card hover:bg-brand-border text-slate-300">🟢 Veg Only</button>
          <button onclick="filterDiet('non-veg')" id="diet-nonveg" class="diet-btn px-3 py-2 rounded-xl text-xs font-semibold bg-brand-card hover:bg-brand-border text-slate-300">🔴 Non-Veg</button>
        </div>
      </div>

      <!-- Brand Filter Tabs -->
      <div class="flex overflow-x-auto space-x-2 pb-4 scrollbar-none mb-8 border-b border-brand-border/40">
        <button onclick="filterMenuByBrand('all')" class="brand-filter-tab px-4 py-2 rounded-xl text-xs font-bold whitespace-nowrap bg-brand-card text-brand-gold border border-brand-gold/40">★ All Virtual Brands</button>
        <button onclick="filterMenuByBrand('burger')" class="brand-filter-tab px-4 py-2 rounded-xl text-xs font-bold whitespace-nowrap bg-brand-card text-slate-300 hover:text-white border border-transparent">Burger Forge 3D</button>
        <button onclick="filterMenuByBrand('biryani')" class="brand-filter-tab px-4 py-2 rounded-xl text-xs font-bold whitespace-nowrap bg-brand-card text-slate-300 hover:text-white border border-transparent">AHA Biryani Express</button>
        <button onclick="filterMenuByBrand('asian')" class="brand-filter-tab px-4 py-2 rounded-xl text-xs font-bold whitespace-nowrap bg-brand-card text-slate-300 hover:text-white border border-transparent">AHA Wok Republic</button>
        <button onclick="filterMenuByBrand('healthy')" class="brand-filter-tab px-4 py-2 rounded-xl text-xs font-bold whitespace-nowrap bg-brand-card text-slate-300 hover:text-white border border-transparent">Green Oasis</button>
      </div>

      <!-- Menu Grid -->
      <div id="menuGrid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
        <!-- Generated by JavaScript dynamically -->
      </div>
    </div>
  </section>

  <!-- LIVE METRICS & HYGIENE STATUS -->
  <section id="metrics" class="py-20 bg-brand-dark/40 border-t border-brand-border/60">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="grid grid-cols-1 lg:grid-cols-12 gap-8 items-center">
        
        <div class="lg:col-span-5 space-y-5">
          <span class="text-brand-saffron text-xs font-bold tracking-widest uppercase">Smart Kitchen Telemetry</span>
          <h2 class="text-3xl sm:text-4xl font-extrabold text-white">Sterile Automation & Real-Time Quality</h2>
          <p class="text-slate-300 text-sm leading-relaxed">
            Every dish prepared in the AHA Cloud Kitchen facility passes through continuous UV sterilizers and temperature sensors to assure pristine food safety before tamper-proof packaging.
          </p>

          <div class="space-y-3 pt-2">
            <div class="flex items-center space-x-3 text-sm text-slate-200">
              <div class="w-8 h-8 rounded-lg bg-emerald-500/10 text-emerald-400 flex items-center justify-center">
                <i data-lucide="shield-check" class="w-4 h-4"></i>
              </div>
              <span>ISO 22000 & HACCP Certified Sterile Cleanroom Hub</span>
            </div>
            <div class="flex items-center space-x-3 text-sm text-slate-200">
              <div class="w-8 h-8 rounded-lg bg-brand-gold/10 text-brand-gold flex items-center justify-center">
                <i data-lucide="thermometer" class="w-4 h-4"></i>
              </div>
              <span>Hot delivery dispatch holding at calibrated 68°C</span>
            </div>
            <div class="flex items-center space-x-3 text-sm text-slate-200">
              <div class="w-8 h-8 rounded-lg bg-brand-saffron/10 text-brand-saffron flex items-center justify-center">
                <i data-lucide="zap" class="w-4 h-4"></i>
              </div>
              <span>AI Cook Optimization reducing customer wait time by 42%</span>
            </div>
          </div>
        </div>

        <div class="lg:col-span-7">
          <div class="grid grid-cols-2 sm:grid-cols-2 gap-4">
            
            <div class="glass-card p-6 rounded-2xl border-l-4 border-l-emerald-500">
              <div class="flex justify-between items-center text-xs text-slate-400">
                <span>HYGIENE SCORE</span>
                <span class="w-2.5 h-2.5 rounded-full bg-emerald-400 animate-ping"></span>
              </div>
              <p class="text-3xl font-extrabold text-white mt-2 font-mono" id="hygieneVal">99.8%</p>
              <p class="text-xs text-emerald-400 mt-1">Medical Grade Sterile Pack</p>
            </div>

            <div class="glass-card p-6 rounded-2xl border-l-4 border-l-brand-saffron">
              <div class="flex justify-between items-center text-xs text-slate-400">
                <span>ACTIVE KITCHEN ORDERS</span>
                <i data-lucide="flame" class="w-4 h-4 text-brand-saffron"></i>
              </div>
              <p class="text-3xl font-extrabold text-white mt-2 font-mono" id="activeOrdersVal">24</p>
              <p class="text-xs text-slate-300 mt-1">Across 8 Woks & 4 Smokers</p>
            </div>

            <div class="glass-card p-6 rounded-2xl border-l-4 border-l-brand-gold">
              <div class="flex justify-between items-center text-xs text-slate-400">
                <span>AVG PREP TIME</span>
                <i data-lucide="clock" class="w-4 h-4 text-brand-gold"></i>
              </div>
              <p class="text-3xl font-extrabold text-white mt-2 font-mono">11m 45s</p>
              <p class="text-xs text-brand-gold mt-1">3.2 mins faster than city avg</p>
            </div>

            <div class="glass-card p-6 rounded-2xl border-l-4 border-l-blue-500">
              <div class="flex justify-between items-center text-xs text-slate-400">
                <span>DELIVERY FLEET RADIUS</span>
                <i data-lucide="map-pin" class="w-4 h-4 text-blue-400"></i>
              </div>
              <p class="text-3xl font-extrabold text-white mt-2 font-mono">8.5 KM</p>
              <p class="text-xs text-slate-300 mt-1">Insulated Electric Express</p>
            </div>

          </div>
        </div>

      </div>
    </div>
  </section>

  <!-- LIVE ORDER TRACKER SIMULATION -->
  <section id="tracking" class="py-20 relative">
    <div class="max-w-5xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="glass-card rounded-3xl p-8 border border-brand-border relative overflow-hidden">
        
        <div class="flex flex-col sm:flex-row sm:items-center justify-between pb-6 border-b border-brand-border/60 gap-4">
          <div>
            <div class="inline-flex items-center space-x-2 text-xs font-bold text-brand-gold mb-1">
              <span class="w-2 h-2 rounded-full bg-brand-gold animate-ping"></span>
              <span>LIVE TRACKING DEMO</span>
            </div>
            <h3 class="text-2xl font-bold text-white">Order #AHA-9824</h3>
            <p class="text-xs text-slate-400">Estimated delivery in <strong class="text-white">18 Minutes</strong></p>
          </div>
          <button onclick="advanceTrackingDemo()" class="px-4 py-2 rounded-xl text-xs font-bold bg-brand-card hover:bg-brand-border border border-brand-border text-brand-gold flex items-center space-x-2 self-start sm:self-auto transition-colors">
            <i data-lucide="fast-forward" class="w-3.5 h-3.5"></i>
            <span>Simulate Next Stage</span>
          </button>
        </div>

        <!-- Stepper Progress Bar -->
        <div class="py-8">
          <div class="grid grid-cols-4 gap-2 relative">
            <div class="step-item text-center">
              <div id="step-circle-1" class="w-10 h-10 mx-auto rounded-full bg-emerald-500 text-brand-charcoal font-bold flex items-center justify-center text-sm shadow-lg mb-2">
                ✓
              </div>
              <p class="text-xs font-bold text-white">Order Received</p>
              <p class="text-[10px] text-slate-400">Sterile Kitchen</p>
            </div>

            <div class="step-item text-center">
              <div id="step-circle-2" class="w-10 h-10 mx-auto rounded-full bg-brand-saffron text-white font-bold flex items-center justify-center text-sm shadow-glow-saffron mb-2 animate-pulse">
                🍳
              </div>
              <p class="text-xs font-bold text-white">Under The Wok</p>
              <p class="text-[10px] text-brand-gold">Chef Sautéing</p>
            </div>

            <div class="step-item text-center opacity-60" id="step-col-3">
              <div id="step-circle-3" class="w-10 h-10 mx-auto rounded-full bg-brand-card border border-brand-border text-slate-400 font-bold flex items-center justify-center text-sm mb-2">
                📦
              </div>
              <p class="text-xs font-bold text-slate-300">Vacuum Sealed</p>
              <p class="text-[10px] text-slate-500">Thermal Pack</p>
            </div>

            <div class="step-item text-center opacity-60" id="step-col-4">
              <div id="step-circle-4" class="w-10 h-10 mx-auto rounded-full bg-brand-card border border-brand-border text-slate-400 font-bold flex items-center justify-center text-sm mb-2">
                🛵
              </div>
              <p class="text-xs font-bold text-slate-300">Out For Delivery</p>
              <p class="text-[10px] text-slate-500">Express Rider</p>
            </div>
          </div>
        </div>

        <div class="p-4 rounded-2xl bg-brand-dark/90 border border-brand-border flex items-center justify-between">
          <div class="flex items-center space-x-3">
            <div class="w-10 h-10 rounded-xl bg-brand-card flex items-center justify-center text-xl">
              👨‍🍳
            </div>
            <div>
              <p class="text-xs font-bold text-white">Sous Chef Rohan M.</p>
              <p class="text-[10px] text-slate-400">Culinary Station 3 • Sealed With UV Guard</p>
            </div>
          </div>
          <span class="text-xs px-3 py-1 rounded-full bg-brand-saffron/15 text-brand-saffron font-bold">100% Sealed</span>
        </div>

      </div>
    </div>
  </section>

  <!-- FLOATING CART DRAWER OVERLAY -->
  <div id="cartDrawer" class="fixed inset-0 z-50 pointer-events-none transition-opacity duration-300 opacity-0">
    <!-- Backdrop -->
    <div id="cartBackdrop" onclick="toggleCartDrawer(false)" class="absolute inset-0 bg-black/70 backdrop-blur-sm transition-opacity"></div>

    <!-- Drawer Body -->
    <div id="cartPanel" class="absolute top-0 right-0 h-full w-full max-w-md bg-brand-dark border-l border-brand-border p-6 flex flex-col shadow-2xl transform translate-x-full transition-transform duration-300 pointer-events-auto">
      
      <!-- Drawer Header -->
      <div class="flex items-center justify-between pb-4 border-b border-brand-border">
        <div class="flex items-center space-x-2">
          <i data-lucide="shopping-bag" class="w-5 h-5 text-brand-saffron"></i>
          <h3 class="text-lg font-bold text-white">Your Cloud Basket</h3>
        </div>
        <button onclick="toggleCartDrawer(false)" class="p-2 rounded-xl text-slate-400 hover:text-white hover:bg-brand-card">
          <i data-lucide="x" class="w-5 h-5"></i>
        </button>
      </div>

      <!-- Free Delivery progress bar -->
      <div class="py-3">
        <div class="flex justify-between text-xs text-slate-300 mb-1">
          <span>Free Express Delivery at $35.00</span>
          <span id="freeDeliveryStatus" class="font-bold text-brand-gold">$12.00 away</span>
        </div>
        <div class="w-full bg-brand-card h-2 rounded-full overflow-hidden">
          <div id="deliveryProgressBar" class="bg-gradient-to-r from-brand-saffron to-brand-gold h-full w-0 transition-all duration-300"></div>
        </div>
      </div>

      <!-- Items List -->
      <div id="cartItemsContainer" class="flex-1 overflow-y-auto space-y-3 py-3 pr-1">
        <!-- Rendered dynamically -->
      </div>

      <!-- Promo Code Field -->
      <div class="pt-3 border-t border-brand-border space-y-3">
        <div class="flex space-x-2">
          <input type="text" id="promoInput" placeholder="Enter coupon (Try: AHA3D)" class="flex-1 bg-brand-card border border-brand-border rounded-xl px-3 py-2 text-xs text-white uppercase focus:outline-none focus:border-brand-gold" />
          <button onclick="applyPromoCode()" class="px-4 py-2 bg-brand-card hover:bg-brand-border text-brand-gold text-xs font-bold rounded-xl border border-brand-border transition-colors">
            Apply
          </button>
        </div>
        <div id="promoNotice" class="text-[11px] text-emerald-400 hidden"></div>

        <!-- Bill Calculation -->
        <div class="space-y-1.5 text-xs text-slate-300 pt-2 border-t border-brand-border/60">
          <div class="flex justify-between">
            <span>Subtotal</span>
            <span id="cartSubtotal" class="font-mono text-white">$0.00</span>
          </div>
          <div class="flex justify-between text-emerald-400" id="discountRow" style="display: none;">
            <span>Discount (Promo)</span>
            <span id="cartDiscount" class="font-mono">-$0.00</span>
          </div>
          <div class="flex justify-between">
            <span>Sterile Packaging & Thermal Bag</span>
            <span class="font-mono text-white">$1.50</span>
          </div>
          <div class="flex justify-between">
            <span>Electric Fleet Delivery</span>
            <span id="cartDeliveryFee" class="font-mono text-white">$2.99</span>
          </div>
          <div class="flex justify-between text-sm font-extrabold text-white pt-2 border-t border-brand-border">
            <span>Total Payable</span>
            <span id="cartGrandTotal" class="font-mono text-brand-gold text-base">$0.00</span>
          </div>
        </div>

        <!-- Checkout Action Button -->
        <button onclick="openCheckoutModal()" id="checkoutBtn" class="w-full py-3.5 rounded-xl font-bold bg-gradient-to-r from-brand-saffron to-brand-gold text-brand-charcoal hover:opacity-95 shadow-glow-saffron transition-all duration-200 flex items-center justify-center space-x-2">
          <span>Proceed To Rapid Checkout</span>
          <i data-lucide="arrow-right" class="w-4 h-4"></i>
        </button>
      </div>

    </div>
  </div>

  <!-- CHECKOUT SUCCESS MODAL -->
  <div id="checkoutModal" class="fixed inset-0 z-50 hidden flex items-center justify-center p-4 bg-black/80 backdrop-blur-md">
    <div class="bg-brand-dark border border-brand-gold/40 rounded-3xl p-6 sm:p-8 max-w-md w-full shadow-2xl space-y-5 text-center">
      <div class="w-16 h-16 rounded-full bg-emerald-500/20 text-emerald-400 flex items-center justify-center mx-auto text-3xl">
        🎉
      </div>
      <div>
        <h3 class="text-2xl font-bold text-white">Order Confirmed!</h3>
        <p class="text-xs text-slate-300 mt-1">Order <strong class="text-brand-gold font-mono">#AHA-7791</strong> has been dispatched to our sterile line.</p>
      </div>
      
      <div class="p-4 rounded-2xl bg-brand-card/70 border border-brand-border text-left text-xs space-y-2">
        <div class="flex justify-between">
          <span class="text-slate-400">Delivery Address:</span>
          <span class="text-white font-medium">Boutique St, Suite 4B</span>
        </div>
        <div class="flex justify-between">
          <span class="text-slate-400">Prep Station:</span>
          <span class="text-white font-medium">Smoker & Wok Bay 2</span>
        </div>
        <div class="flex justify-between">
          <span class="text-slate-400">Est. Arrival:</span>
          <span class="text-brand-gold font-bold">19 Minutes</span>
        </div>
      </div>

      <button onclick="closeCheckoutModal()" class="w-full py-3 rounded-xl font-bold bg-brand-card hover:bg-brand-border text-white transition-all text-xs">
        Return to Culinary Experience
      </button>
    </div>
  </div>

  <!-- TOAST NOTIFICATION CONTAINER -->
  <div id="toastContainer" class="fixed bottom-6 right-6 z-50 space-y-2 pointer-events-none"></div>

  <!-- FOOTER -->
  <footer class="bg-brand-dark border-t border-brand-border/80 py-12 text-xs text-slate-400">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 grid grid-cols-1 md:grid-cols-4 gap-8">
      
      <div class="space-y-3">
        <div class="flex items-center space-x-2">
          <div class="w-8 h-8 rounded-xl bg-brand-saffron flex items-center justify-center text-brand-charcoal font-bold">
            <i data-lucide="flame" class="w-4 h-4 fill-current"></i>
          </div>
          <span class="text-lg font-serif font-bold text-white">AHA Kitchens</span>
        </div>
        <p class="text-slate-400 leading-relaxed">
          Pioneering cloud culinary architectures with contactless hygiene, curated gastronomy, and rapid 3D previews.
        </p>
      </div>

      <div>
        <h4 class="text-white font-bold mb-3 uppercase tracking-wider text-[11px]">Virtual Houses</h4>
        <ul class="space-y-2">
          <li><a href="#menu" class="hover:text-brand-gold transition-colors">AHA Biryani Express</a></li>
          <li><a href="#menu" class="hover:text-brand-gold transition-colors">Burger Forge 3D</a></li>
          <li><a href="#menu" class="hover:text-brand-gold transition-colors">AHA Wok Republic</a></li>
          <li><a href="#menu" class="hover:text-brand-gold transition-colors">Green Oasis Superfoods</a></li>
        </ul>
      </div>

      <div>
        <h4 class="text-white font-bold mb-3 uppercase tracking-wider text-[11px]">Contact & Locations</h4>
        <ul class="space-y-2">
          <li>Central Hub: 404 Gastronomy Way</li>
          <li>Hotline: +1 (800) AHA-MEAL</li>
          <li>Support: chef@ahakitchens.internal</li>
          <li>Kitchen Hours: 11:00 AM – 3:30 AM Daily</li>
        </ul>
      </div>

      <div>
        <h4 class="text-white font-bold mb-3 uppercase tracking-wider text-[11px]">Sterile Certification</h4>
        <p class="leading-relaxed">
          Inspected daily by certified food hygiene officers. All packages arrive with sterile tamper indicator seals.
        </p>
        <div class="mt-3 inline-block px-3 py-1 bg-emerald-500/10 text-emerald-400 border border-emerald-500/30 rounded-full font-mono text-[10px]">
          100% CONTACTLESS DISPATCH
        </div>
      </div>

    </div>
    
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 mt-10 pt-6 border-t border-brand-border/40 text-center text-[11px] text-slate-500">
      © 2026 AHA Cloud Kitchen Technologies. All rights reserved. Crafting the future of taste.
    </div>
  </footer>

  <script>
    /* ==========================================================================
       THREE.JS 3D SCENE & PROCEDURAL FOOD MODEL ENGINE
       ========================================================================== */
    let scene, camera, renderer, controls;
    let currentDishGroup;
    let steamParticles, spiceParticles;
    let isExploded = false;
    let activeDishType = 'burger';
    let soundEnabled = true;
    let animationFrameId;

    // Audio synthesizer using Web Audio API for interactive clicks
    let audioCtx;
    function playUiSound(frequency = 520, type = 'sine', duration = 0.08) {
      if (!soundEnabled) return;
      try {
        if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        if (audioCtx.state === 'suspended') audioCtx.resume();
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.type = type;
        osc.frequency.setValueAtTime(frequency, audioCtx.currentTime);
        gain.gain.setValueAtTime(0.08, audioCtx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + duration);
        osc.connect(gain);
        gain.connect(audioCtx.destination);
        osc.start();
        osc.stop(audioCtx.currentTime + duration);
      } catch (e) {
        // Fallback gracefully if blocked by autoplay policy
      }
    }

    function toggleSound() {
      soundEnabled = !soundEnabled;
      const icon = document.getElementById('soundIcon');
      if (soundEnabled) {
        icon.setAttribute('data-lucide', 'volume-2');
        showToast('Sound Effects Enabled 🔊');
        playUiSound(600);
      } else {
        icon.setAttribute('data-lucide', 'volume-x');
        showToast('Sound Effects Muted 🔇');
      }
      lucide.createIcons();
    }

    // Initialize 3D Engine
    function init3DScene() {
      const container = document.getElementById('threeContainer');
      const width = container.clientWidth;
      const height = container.clientHeight;

      // 1. Scene
      scene = new THREE.Scene();
      scene.fog = new THREE.FogExp2(0x0B0F17, 0.035);

      // 2. Camera
      camera = new THREE.PerspectiveCamera(45, width / height, 0.1, 100);
      camera.position.set(0, 3.2, 7.8);

      // 3. Renderer
      renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
      renderer.setSize(width, height);
      renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
      renderer.shadowMap.enabled = true;
      renderer.shadowMap.type = THREE.PCFSoftShadowMap;
      renderer.toneMapping = THREE.ACESFilmicToneMapping;
      renderer.toneMappingExposure = 1.15;
      container.appendChild(renderer.domElement);

      // 4. Controls
      controls = new THREE.OrbitControls(camera, renderer.domElement);
      controls.enableDamping = true;
      controls.dampingFactor = 0.05;
      controls.maxPolarAngle = Math.PI / 2 + 0.08; // don't go below floor
      controls.minDistance = 4;
      controls.maxDistance = 12;
      controls.target.set(0, 0.4, 0);

      // 5. Lights
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.85);
      scene.add(ambientLight);

      const mainSpot = new THREE.SpotLight(0xffa834, 2.8);
      mainSpot.position.set(5, 8, 5);
      mainSpot.angle = Math.PI / 4;
      mainSpot.penumbra = 0.5;
      mainSpot.castShadow = true;
      scene.add(mainSpot);

      const rimLight = new THREE.DirectionalLight(0x00e1ff, 1.2);
      rimLight.position.set(-6, 4, -4);
      scene.add(rimLight);

      const warmFill = new THREE.PointLight(0xff4500, 1.5, 10);
      warmFill.position.set(0, -1, 3);
      scene.add(warmFill);

      // 6. Base Pedestal / Serving Platter
      createPedestal();

      // 7. Ambient Floating Particles (Steam & Spices)
      createParticleAtmosphere();

      // 8. Build initial default dish model (Burger Forge)
      currentDishGroup = new THREE.Group();
      scene.add(currentDishGroup);
      buildBurgerModel();

      // Window resize listener
      window.addEventListener('resize', onWindowResize);

      // Mouse Parallax movement
      document.addEventListener('mousemove', (e) => {
        const mouseX = (e.clientX / window.innerWidth) - 0.5;
        const mouseY = (e.clientY / window.innerHeight) - 0.5;
        if (currentDishGroup) {
          currentDishGroup.rotation.y += mouseX * 0.005;
          currentDishGroup.rotation.x = Math.max(-0.2, Math.min(0.2, currentDishGroup.rotation.x + mouseY * 0.003));
        }
      });

      // Start Animation Loop
      animate3D();
    }

    // Pedestal Plate
    function createPedestal() {
      const platterGeo = new THREE.CylinderGeometry(2.7, 2.5, 0.16, 48);
      const platterMat = new THREE.MeshStandardMaterial({
        color: 0x18202F,
        roughness: 0.35,
        metalness: 0.7,
      });
      const platter = new THREE.Mesh(platterGeo, platterMat);
      platter.position.y = -0.9;
      platter.receiveShadow = true;
      scene.add(platter);

      // Accent Glowing Ring
      const ringGeo = new THREE.TorusGeometry(2.75, 0.03, 16, 64);
      const ringMat = new THREE.MeshBasicMaterial({ color: 0xFF5400 });
      const ring = new THREE.Mesh(ringGeo, ringMat);
      ring.rotation.x = Math.PI / 2;
      ring.position.y = -0.84;
      scene.add(ring);
    }

    // Steam & Floating Spices
    function createParticleAtmosphere() {
      // Steam
      const particleCount = 45;
      const steamGeo = new THREE.BufferGeometry();
      const steamPositions = new Float32Array(particleCount * 3);

      for (let i = 0; i < particleCount * 3; i += 3) {
        steamPositions[i] = (Math.random() - 0.5) * 1.5;
        steamPositions[i + 1] = Math.random() * 2.8;
        steamPositions[i + 2] = (Math.random() - 0.5) * 1.5;
      }
      steamGeo.setAttribute('position', new THREE.BufferAttribute(steamPositions, 3));

      const steamMat = new THREE.PointsMaterial({
        color: 0xffffff,
        size: 0.12,
        transparent: true,
        opacity: 0.28,
        blending: THREE.AdditiveBlending
      });
      steamParticles = new THREE.Points(steamGeo, steamMat);
      scene.add(steamParticles);
    }

    // Model 1: Procedural Artisanal Burger
    function buildBurgerModel() {
      clearDishGroup();
      isExploded = false;
      document.getElementById('explodeBtnText').innerText = "Deconstruct Layers";

      const burger = new THREE.Group();

      // Materials
      const bunMat = new THREE.MeshStandardMaterial({ color: 0xD78B3F, roughness: 0.6 });
      const pattyMat = new THREE.MeshStandardMaterial({ color: 0x3E2319, roughness: 0.85, bumpScale: 0.05 });
      const cheeseMat = new THREE.MeshStandardMaterial({ color: 0xFFA500, roughness: 0.4 });
      const lettuceMat = new THREE.MeshStandardMaterial({ color: 0x34A853, roughness: 0.5 });
      const tomatoMat = new THREE.MeshStandardMaterial({ color: 0xD32F2F, roughness: 0.35 });

      // 1. Bottom Bun
      const bottomBunGeo = new THREE.CylinderGeometry(1.2, 1.15, 0.4, 32);
      const bottomBun = new THREE.Mesh(bottomBunGeo, bunMat);
      bottomBun.position.y = -0.55;
      bottomBun.castShadow = true;
      bottomBun.userData = { defaultY: -0.55, explodedY: -1.2 };
      burger.add(bottomBun);

      // 2. Angus Patty
      const pattyGeo = new THREE.CylinderGeometry(1.26, 1.24, 0.45, 32);
      const patty = new THREE.Mesh(pattyGeo, pattyMat);
      patty.position.y = -0.15;
      patty.castShadow = true;
      patty.userData = { defaultY: -0.15, explodedY: -0.5 };
      burger.add(patty);

      // 3. Melted Cheddar Slice (Rotated Square)
      const cheeseGeo = new THREE.BoxGeometry(1.6, 0.05, 1.6);
      const cheese = new THREE.Mesh(cheeseGeo, cheeseMat);
      cheese.position.y = 0.12;
      cheese.rotation.y = Math.PI / 4;
      cheese.userData = { defaultY: 0.12, explodedY: 0.1 };
      burger.add(cheese);

      // 4. Curly Crisp Lettuce
      const lettuceGeo = new THREE.CylinderGeometry(1.36, 1.3, 0.1, 16);
      const lettuce = new THREE.Mesh(lettuceGeo, lettuceMat);
      lettuce.position.y = 0.22;
      lettuce.userData = { defaultY: 0.22, explodedY: 0.6 };
      burger.add(lettuce);

      // 5. Tomato Slices (2 Slices)
      const tomatoGeo = new THREE.CylinderGeometry(0.55, 0.55, 0.1, 24);
      const t1 = new THREE.Mesh(tomatoGeo, tomatoMat);
      t1.position.set(-0.4, 0.32, 0.1);
      const t2 = new THREE.Mesh(tomatoGeo, tomatoMat);
      t2.position.set(0.4, 0.32, -0.1);
      const tomatoGroup = new THREE.Group();
      tomatoGroup.add(t1);
      tomatoGroup.add(t2);
      tomatoGroup.userData = { defaultY: 0, explodedY: 1.1 };
      burger.add(tomatoGroup);

      // 6. Brioche Top Bun (Hemisphere)
      const topBunGeo = new THREE.SphereGeometry(1.25, 32, 16, 0, Math.PI * 2, 0, Math.PI / 2);
      const topBun = new THREE.Mesh(topBunGeo, bunMat);
      topBun.position.y = 0.4;
      topBun.castShadow = true;
      topBun.userData = { defaultY: 0.4, explodedY: 1.8 };

      // Sesame seeds scattered on top bun
      const seedMat = new THREE.MeshBasicMaterial({ color: 0xFFF3D6 });
      const seedGeo = new THREE.SphereGeometry(0.035, 6, 6);
      for (let i = 0; i < 35; i++) {
        const seed = new THREE.Mesh(seedGeo, seedMat);
        const theta = Math.random() * Math.PI * 2;
        const phi = Math.random() * (Math.PI / 3);
        seed.position.x = 1.25 * Math.sin(phi) * Math.cos(theta);
        seed.position.y = 1.25 * Math.cos(phi) - 0.05;
        seed.position.z = 1.25 * Math.sin(phi) * Math.sin(theta);
        topBun.add(seed);
      }
      burger.add(topBun);

      currentDishGroup.add(burger);
      updateHotspotInfo(
        'Signature Burger',
        'Burger Forge 3D Smash Double',
        'Double Angus beef patty, hand-caramelized sweet shallots, melted Oregon cheddar, crisp artisanal lettuce, and house smoky aioli on toasted brioche.'
      );
    }

    // Model 2: Procedural Dum Royal Biryani Clay Handi Pot
    function buildBiryaniModel() {
      clearDishGroup();
      isExploded = false;
      document.getElementById('explodeBtnText').innerText = "Deconstruct Flavors";

      const biryani = new THREE.Group();

      // Handi Clay Pot
      const potMat = new THREE.MeshStandardMaterial({ color: 0x93482A, roughness: 0.85 });
      const potGeo = new THREE.CylinderGeometry(1.6, 1.1, 1.2, 32);
      const pot = new THREE.Mesh(potGeo, potMat);
      pot.position.y = -0.3;
      pot.castShadow = true;
      pot.userData = { defaultY: -0.3, explodedY: -0.8 };
      biryani.add(pot);

      // Gold Rim Band
      const bandGeo = new THREE.TorusGeometry(1.62, 0.04, 16, 32);
      const bandMat = new THREE.MeshStandardMaterial({ color: 0xFFA500, metalness: 0.8, roughness: 0.3 });
      const band = new THREE.Mesh(bandGeo, bandMat);
      band.rotation.x = Math.PI / 2;
      band.position.y = 0.28;
      band.userData = { defaultY: 0.28, explodedY: -0.2 };
      biryani.add(band);

      // Golden Rice Grains Mound
      const riceGeo = new THREE.SphereGeometry(1.48, 24, 16, 0, Math.PI * 2, 0, Math.PI / 2.2);
      const riceMat = new THREE.MeshStandardMaterial({ color: 0xF3C952, roughness: 0.9, flatShading: true });
      const rice = new THREE.Mesh(riceGeo, riceMat);
      rice.position.y = 0.2;
      rice.userData = { defaultY: 0.2, explodedY: 0.5 };
      biryani.add(rice);

      // Mint / Coriander Herbs & Saffron Strands
      const herbGeo = new THREE.DodecahedronGeometry(0.12);
      const herbMat = new THREE.MeshStandardMaterial({ color: 0x2E7D32, roughness: 0.5 });
      const herbGroup = new THREE.Group();
      for (let i = 0; i < 15; i++) {
        const herb = new THREE.Mesh(herbGeo, herbMat);
        herb.position.set((Math.random() - 0.5) * 1.5, 0.85 + Math.random() * 0.2, (Math.random() - 0.5) * 1.5);
        herb.rotation.set(Math.random(), Math.random(), Math.random());
        herbGroup.add(herb);
      }
      herbGroup.userData = { defaultY: 0, explodedY: 1.2 };
      biryani.add(herbGroup);

      // Roasted Chicken Shank or Dum Garnish (Procedural Capsule)
      const shankGeo = new THREE.CylinderGeometry(0.2, 0.32, 1.1, 16);
      const shankMat = new THREE.MeshStandardMaterial({ color: 0x6E2C00, roughness: 0.7 });
      const shank = new THREE.Mesh(shankGeo, shankMat);
      shank.rotation.z = 0.6;
      shank.position.set(0.4, 0.8, 0);
      shank.userData = { defaultY: 0.8, explodedY: 1.6 };
      biryani.add(shank);

      currentDishGroup.add(biryani);
      updateHotspotInfo(
        'Royal Dum Special',
        'AHA Hyderabadi Gosht Biryani',
        'Aged Long-grain Basmati steamed sealed in terracotta handi with real Kashmiri saffron strands, brown onions (birista), and slow-cooked marinated shank.'
      );
    }

    // Model 3: Glowing Golden Luxury Cloche
    function buildClocheModel() {
      clearDishGroup();
      isExploded = false;
      document.getElementById('explodeBtnText').innerText = "Lift Mystery Cloche";

      const clocheGroup = new THREE.Group();

      // Shiny Gold Dome
      const domeMat = new THREE.MeshStandardMaterial({
        color: 0xDFAC38,
        metalness: 0.9,
        roughness: 0.2,
      });
      const domeGeo = new THREE.SphereGeometry(1.65, 32, 24, 0, Math.PI * 2, 0, Math.PI / 2);
      const dome = new THREE.Mesh(domeGeo, domeMat);
      dome.position.y = -0.5;
      dome.userData = { defaultY: -0.5, explodedY: 1.8 };
      dome.castShadow = true;

      // Handle finial
      const finialGeo = new THREE.SphereGeometry(0.24, 16, 16);
      const finial = new THREE.Mesh(finialGeo, domeMat);
      finial.position.y = 1.65;
      dome.add(finial);

      clocheGroup.add(dome);

      // Glowing Inner Dish Reveal
      const innerGeo = new THREE.TorusKnotGeometry(0.55, 0.16, 64, 16);
      const innerMat = new THREE.MeshStandardMaterial({
        color: 0xFF5400,
        emissive: 0xFF2200,
        roughness: 0.2,
        metalness: 0.5
      });
      const innerTreasure = new THREE.Mesh(innerGeo, innerMat);
      innerTreasure.position.y = -0.2;
      innerTreasure.userData = { defaultY: -0.2, explodedY: -0.2 };
      clocheGroup.add(innerTreasure);

      currentDishGroup.add(clocheGroup);
      updateHotspotInfo(
        'Chef Mystery Reserve',
        'Signature Cloche Surprise',
        'A confidential rotating degustation masterpiece from our Michelin-vetted test kitchen. Unveiled daily at 7:00 PM.'
      );
    }

    function clearDishGroup() {
      while (currentDishGroup.children.length > 0) {
        currentDishGroup.remove(currentDishGroup.children[0]);
      }
    }

    // Dish Switcher UI handler
    function switchDishModel(type) {
      activeDishType = type;
      playUiSound(700);

      // Update button tabs
      document.querySelectorAll('.dish-selector-btn').forEach(btn => {
        btn.classList.remove('bg-brand-saffron', 'text-white');
        btn.classList.add('bg-brand-card', 'text-slate-300');
      });
      const currentTab = document.getElementById(`dishTab-${type}`);
      if (currentTab) {
        currentTab.classList.remove('bg-brand-card', 'text-slate-300');
        currentTab.classList.add('bg-brand-saffron', 'text-white');
      }

      const label = document.getElementById('current3DLabel');
      if (type === 'burger') {
        label.innerText = 'Smash Burger Loaded Deluxe';
        buildBurgerModel();
      } else if (type === 'biryani') {
        label.innerText = 'AHA Dum Royal Biryani';
        buildBiryaniModel();
      } else if (type === 'cloche') {
        label.innerText = 'Sterile Cloche Reveal Reserve';
        buildClocheModel();
      }
    }

    // Deconstructed / Exploded view toggle
    function toggleExplodedView() {
      isExploded = !isExploded;
      playUiSound(isExploded ? 850 : 440);
      document.getElementById('explodeBtnText').innerText = isExploded ? "Assemble Dish" : "Deconstruct Layers";
      showToast(isExploded ? "Deconstructed view engaged 🔬" : "Layers assembled");
    }

    // Reset 3D Camera position
    function reset3DCamera() {
      playUiSound(500);
      controls.reset();
      camera.position.set(0, 3.2, 7.8);
      controls.target.set(0, 0.4, 0);
    }

    // Dynamic Hotspot Info Box updater
    function updateHotspotInfo(category, title, desc) {
      document.getElementById('hotspotCategory').innerText = category;
      document.getElementById('hotspotTitle').innerText = title;
      document.getElementById('hotspotDesc').innerText = desc;
    }

    // 3D Animation Loop
    function animate3D() {
      animationFrameId = requestAnimationFrame(animate3D);

      // Smooth idle rotation
      if (currentDishGroup) {
        currentDishGroup.rotation.y += 0.005;

        // Smoothly interpolate exploded layer positions
        currentDishGroup.traverse((child) => {
          if (child.userData && child.userData.defaultY !== undefined) {
            const targetY = isExploded ? child.userData.explodedY : child.userData.defaultY;
            child.position.y += (targetY - child.position.y) * 0.1;
          }
        });
      }

      // Animate Steam Particles rising
      if (steamParticles) {
        const positions = steamParticles.geometry.attributes.position.array;
        for (let i = 1; i < positions.length; i += 3) {
          positions[i] += 0.015;
          if (positions[i] > 3.2) {
            positions[i] = 0.2;
          }
        }
        steamParticles.geometry.attributes.position.needsUpdate = true;
      }

      controls.update();
      renderer.render(scene, camera);
    }

    function onWindowResize() {
      const container = document.getElementById('threeContainer');
      if (!container || !renderer || !camera) return;
      const width = container.clientWidth;
      const height = container.clientHeight;
      camera.aspect = width / height;
      camera.updateProjectionMatrix();
      renderer.setSize(width, height);
    }

    /* ==========================================================================
       MENU DATABASE & E-COMMERCE CART ENGINE
       ========================================================================== */
    const MENU_DATABASE = [
      {
        id: 'bf-1',
        brand: 'burger',
        brandName: 'Burger Forge 3D',
        name: 'The 450° Truffle Smash Burger',
        price: 13.99,
        category: 'Burgers',
        diet: 'non-veg',
        spiciness: 1,
        calories: 780,
        prepMins: 9,
        badge: 'Bestseller',
        desc: 'Double Angus smashed patties, molten aged cheddar, black summer truffle aioli, toasted artisanal milk bun.',
        rating: 4.9,
        modelType: 'burger'
      },
      {
        id: 'bf-2',
        brand: 'burger',
        brandName: 'Burger Forge 3D',
        name: 'Crispy Sriracha Ghost Chicken',
        price: 12.49,
        category: 'Burgers',
        diet: 'non-veg',
        spiciness: 3,
        calories: 840,
        prepMins: 11,
        badge: 'Fiery',
        desc: 'Buttermilk-brined crispy fried chicken thigh dipped in ghost-pepper glaze, pickled dill crunch, cool slaw.',
        rating: 4.8,
        modelType: 'burger'
      },
      {
        id: 'by-1',
        brand: 'biryani',
        brandName: 'AHA Biryani Express',
        name: 'Royal Hyderabadi Mutton Dum',
        price: 17.99,
        category: 'Biryani',
        diet: 'non-veg',
        spiciness: 2,
        calories: 920,
        prepMins: 14,
        badge: 'Chef Special',
        desc: 'Sealed dough dum clay pot basmati with tender fall-off-the-bone spiced goat meat, saffron, and fresh mint.',
        rating: 5.0,
        modelType: 'biryani'
      },
      {
        id: 'by-2',
        brand: 'biryani',
        brandName: 'AHA Biryani Express',
        name: 'Smoked Paneer Tikka Biryani',
        price: 14.50,
        category: 'Biryani',
        diet: 'veg',
        spiciness: 2,
        calories: 740,
        prepMins: 12,
        badge: 'Pure Veg',
        desc: 'Charcoal-grilled malai paneer cubes, layered with aged fragrant basmati rice and royal ground cardamom masala.',
        rating: 4.7,
        modelType: 'biryani'
      },
      {
        id: 'wk-1',
        brand: 'asian',
        brandName: 'AHA Wok Republic',
        name: 'Fiery Sichuan Dan-Dan Noodles',
        price: 14.25,
        category: 'Asian',
        diet: 'non-veg',
        spiciness: 3,
        calories: 680,
        prepMins: 8,
        badge: 'Wok Seared',
        desc: 'Hand-pulled wheat noodles coated in chili oil, Sichuan peppercorn sauce, minced pork, and bok choy.',
        rating: 4.9,
        modelType: 'cloche'
      },
      {
        id: 'wk-2',
        brand: 'asian',
        brandName: 'AHA Wok Republic',
        name: 'Truffle & Edamame Crystal Dim Sums',
        price: 11.99,
        category: 'Asian',
        diet: 'veg',
        spiciness: 1,
        calories: 360,
        prepMins: 10,
        badge: 'Steamed',
        desc: 'Translucent steamed dumplings stuffed with young edamame, wild mushrooms, and fragrant white truffle essence.',
        rating: 4.8,
        modelType: 'cloche'
      },
      {
        id: 'go-1',
        brand: 'healthy',
        brandName: 'Green Oasis',
        name: 'Avocado Glow Quinoa Bowl',
        price: 13.50,
        category: 'Bowls',
        diet: 'veg',
        spiciness: 1,
        calories: 490,
        prepMins: 7,
        badge: 'Superfood',
        desc: 'Hass avocado, tri-color organic quinoa, charred edamame, shaved radishes, pumpkin seeds, and ginger-lime tahini.',
        rating: 4.9,
        modelType: 'burger'
      },
      {
        id: 'go-2',
        brand: 'healthy',
        brandName: 'Green Oasis',
        name: 'Cold-Pressed Golden Turmeric Elixir',
        price: 6.99,
        category: 'Drinks',
        diet: 'veg',
        spiciness: 1,
        calories: 120,
        prepMins: 2,
        badge: 'Raw & Fresh',
        desc: 'Cold pressed valencia oranges, wild turmeric root, ginger shot, and organic black pepper oil for high absorption.',
        rating: 4.7,
        modelType: 'cloche'
      }
    ];

    let currentBrandFilter = 'all';
    let currentDietFilter = 'all';
    let searchQuery = '';
    let cart = [];
    let appliedDiscount = 0; // %

    // Render Menu Items
    function renderMenu() {
      const grid = document.getElementById('menuGrid');
      grid.innerHTML = '';

      const filtered = MENU_DATABASE.filter(item => {
        const matchesBrand = (currentBrandFilter === 'all' || item.brand === currentBrandFilter);
        const matchesDiet = (currentDietFilter === 'all' || item.diet === currentDietFilter);
        const matchesSearch = item.name.toLowerCase().includes(searchQuery.toLowerCase()) || 
                              item.desc.toLowerCase().includes(searchQuery.toLowerCase()) ||
                              item.brandName.toLowerCase().includes(searchQuery.toLowerCase());
        return matchesBrand && matchesDiet && matchesSearch;
      });

      if (filtered.length === 0) {
        grid.innerHTML = `
          <div class="col-span-full text-center py-12 text-slate-400">
            <p class="text-3xl mb-2">🔍</p>
            <p class="font-bold text-white">No culinary items match your filter criteria.</p>
            <p class="text-xs mt-1">Try resetting search filters or explore another brand kitchen.</p>
          </div>
        `;
        return;
      }

      filtered.forEach(item => {
        const isVeg = item.diet === 'veg';
        const spiceIcons = '🌶️'.repeat(item.spiciness);

        const card = document.createElement('div');
        card.className = "glass-card rounded-2xl p-5 border border-brand-border/80 hover:border-brand-saffron/40 transition-all duration-300 flex flex-col justify-between group";

        card.innerHTML = `
          <div>
            <!-- Header Badges -->
            <div class="flex items-center justify-between mb-3">
              <span class="text-[10px] font-bold uppercase tracking-wider px-2 py-0.5 rounded-full ${isVeg ? 'bg-emerald-500/20 text-emerald-400 border border-emerald-500/30' : 'bg-red-500/20 text-red-400 border border-red-500/30'} flex items-center space-x-1">
                <span>${isVeg ? '🟢 VEG' : '🔴 NON-VEG'}</span>
              </span>
              <span class="text-xs text-brand-gold font-bold flex items-center space-x-1">
                <span>★</span>
                <span>${item.rating}</span>
              </span>
            </div>

            <!-- Title & Brand -->
            <p class="text-[11px] font-semibold text-brand-gold/90">${item.brandName}</p>
            <h3 class="text-lg font-bold text-white group-hover:text-brand-saffron transition-colors mt-0.5 leading-snug">${item.name}</h3>
            <p class="text-xs text-slate-400 mt-2 line-clamp-2 leading-relaxed">${item.desc}</p>
            
            <!-- Quick Specs -->
            <div class="flex items-center space-x-3 text-[11px] text-slate-400 mt-3 pt-3 border-t border-brand-border/60">
              <span>⏱ ${item.prepMins}m Prep</span>
              <span>🔥 ${item.calories} kcal</span>
              <span title="Spice Level">${spiceIcons}</span>
            </div>
          </div>

          <!-- Bottom Price & Cart Action -->
          <div class="mt-4 pt-4 border-t border-brand-border/60 flex items-center justify-between">
            <div>
              <span class="text-[10px] uppercase text-slate-400 block font-semibold">Price</span>
              <span class="text-lg font-extrabold text-white font-mono">$${item.price.toFixed(2)}</span>
            </div>

            <div class="flex items-center space-x-2">
              <button onclick="inspectIn3D('${item.modelType}')" title="Preview in 3D" class="p-2.5 rounded-xl bg-brand-card hover:bg-brand-border text-slate-300 border border-brand-border transition-colors">
                <i data-lucide="eye" class="w-4 h-4"></i>
              </button>
              <button onclick="addToCart('${item.id}')" class="px-3.5 py-2.5 rounded-xl bg-gradient-to-r from-brand-saffron to-brand-gold hover:opacity-95 text-brand-charcoal text-xs font-bold shadow-glow-saffron transition-transform active:scale-95 flex items-center space-x-1.5">
                <i data-lucide="plus" class="w-3.5 h-3.5"></i>
                <span>Add</span>
              </button>
            </div>
          </div>
        `;
        grid.appendChild(card);
      });

      lucide.createIcons();
    }

    // Inspect dish right away in the 3D scene
    function inspectIn3D(modelType) {
      switchDishModel(modelType);
      const heroSec = document.getElementById('hero');
      heroSec.scrollIntoView({ behavior: 'smooth' });
      showToast('Inspecting 3D model in live viewport ✨');
    }

    // Filter Handlers
    function filterMenuByBrand(brandKey) {
      currentBrandFilter = brandKey;
      playUiSound(540);

      document.querySelectorAll('.brand-filter-tab').forEach(tab => {
        tab.classList.remove('text-brand-gold', 'border-brand-gold/40');
        tab.classList.add('text-slate-300', 'border-transparent');
      });

      if (event && event.target) {
        event.target.classList.remove('text-slate-300', 'border-transparent');
        event.target.classList.add('text-brand-gold', 'border-brand-gold/40');
      }

      renderMenu();
    }

    function filterDiet(dietKey) {
      currentDietFilter = dietKey;
      playUiSound(520);

      document.querySelectorAll('.diet-btn').forEach(btn => {
        btn.classList.remove('bg-brand-saffron', 'text-white');
        btn.classList.add('bg-brand-card', 'text-slate-300');
      });

      const current = document.getElementById(`diet-${dietKey.replace('-', '')}`);
      if (current) {
        current.classList.remove('bg-brand-card', 'text-slate-300');
        current.classList.add('bg-brand-saffron', 'text-white');
      }

      renderMenu();
    }

    function searchMenuItems() {
      searchQuery = document.getElementById('menuSearchInput').value;
      renderMenu();
    }

    /* ==========================================================================
       CART & CHECKOUT LOGIC
       ========================================================================== */
    function addToCart(itemId) {
      playUiSound(880);
      const item = MENU_DATABASE.find(x => x.id === itemId);
      if (!item) return;

      const existing = cart.find(x => x.id === itemId);
      if (existing) {
        existing.quantity += 1;
      } else {
        cart.push({ ...item, quantity: 1 });
      }

      showToast(`Added ${item.name} to basket 🛒`);
      updateCartUI();
    }

    function updateCartItemQty(itemId, delta) {
      playUiSound(delta > 0 ? 600 : 420);
      const item = cart.find(x => x.id === itemId);
      if (!item) return;

      item.quantity += delta;
      if (item.quantity <= 0) {
        cart = cart.filter(x => x.id !== itemId);
      }
      updateCartUI();
    }

    function updateCartUI() {
      const container = document.getElementById('cartItemsContainer');
      const badge = document.getElementById('cartCountBadge');
      const subtotalEl = document.getElementById('cartSubtotal');
      const totalEl = document.getElementById('cartGrandTotal');
      const progressEl = document.getElementById('deliveryProgressBar');
      const freeDelText = document.getElementById('freeDeliveryStatus');

      const totalItems = cart.reduce((acc, curr) => acc + curr.quantity, 0);
      badge.innerText = totalItems;

      container.innerHTML = '';

      if (cart.length === 0) {
        container.innerHTML = `
          <div class="h-64 flex flex-col items-center justify-center text-slate-500 text-center">
            <span class="text-4xl mb-2">🥡</span>
            <p class="text-sm font-semibold text-slate-300">Your basket is currently empty.</p>
            <p class="text-xs text-slate-500 mt-1">Add items from any virtual restaurant brand!</p>
          </div>
        `;
        subtotalEl.innerText = '$0.00';
        totalEl.innerText = '$0.00';
        progressEl.style.width = '0%';
        freeDelText.innerText = '$35.00 away';
        return;
      }

      let subtotal = 0;

      cart.forEach(item => {
        const itemTotal = item.price * item.quantity;
        subtotal += itemTotal;

        const row = document.createElement('div');
        row.className = "flex items-center justify-between p-3 rounded-xl bg-brand-card/70 border border-brand-border/70 text-xs";
        row.innerHTML = `
          <div class="flex-1 pr-2">
            <p class="font-bold text-white">${item.name}</p>
            <p class="text-[10px] text-slate-400 font-mono">$${item.price.toFixed(2)} each</p>
          </div>
          <div class="flex items-center space-x-2">
            <button onclick="updateCartItemQty('${item.id}', -1)" class="w-6 h-6 rounded-lg bg-brand-dark hover:bg-brand-border text-white flex items-center justify-center font-bold font-mono">-</button>
            <span class="font-mono text-xs font-bold text-white w-4 text-center">${item.quantity}</span>
            <button onclick="updateCartItemQty('${item.id}', 1)" class="w-6 h-6 rounded-lg bg-brand-dark hover:bg-brand-border text-white flex items-center justify-center font-bold font-mono">+</button>
          </div>
          <div class="w-14 text-right font-mono font-bold text-brand-gold ml-2">
            $${itemTotal.toFixed(2)}
          </div>
        `;
        container.appendChild(row);
      });

      // Free delivery progress
      const targetFree = 35.00;
      const progressPct = Math.min(100, (subtotal / targetFree) * 100);
      progressEl.style.width = `${progressPct}%`;
      const diff = targetFree - subtotal;
      freeDelText.innerText = diff > 0 ? `$${diff.toFixed(2)} away` : "Unlocked! 🎉";

      // Fees
      const sterilePackFee = 1.50;
      const deliveryFee = (subtotal >= targetFree || subtotal === 0) ? 0.00 : 2.99;
      document.getElementById('cartDeliveryFee').innerText = deliveryFee === 0 ? "FREE" : `$${deliveryFee.toFixed(2)}`;

      // Discount
      const discountVal = subtotal * appliedDiscount;
      if (appliedDiscount > 0) {
        document.getElementById('discountRow').style.display = 'flex';
        document.getElementById('cartDiscount').innerText = `-$${discountVal.toFixed(2)}`;
      } else {
        document.getElementById('discountRow').style.display = 'none';
      }

      const grandTotal = Math.max(0, subtotal - discountVal + sterilePackFee + deliveryFee);
      subtotalEl.innerText = `$${subtotal.toFixed(2)}`;
      totalEl.innerText = `$${grandTotal.toFixed(2)}`;
    }

    function toggleCartDrawer(open) {
      playUiSound(open ? 620 : 400);
      const drawer = document.getElementById('cartDrawer');
      const panel = document.getElementById('cartPanel');
      if (open) {
        drawer.classList.remove('pointer-events-none', 'opacity-0');
        drawer.classList.add('opacity-100');
        panel.classList.remove('translate-x-full');
      } else {
        drawer.classList.add('opacity-0');
        panel.classList.add('translate-x-full');
        setTimeout(() => {
          drawer.classList.add('pointer-events-none');
        }, 300);
      }
    }

    // Promo code handler
    function applyPromoCode() {
      const code = document.getElementById('promoInput').value.trim().toUpperCase();
      const notice = document.getElementById('promoNotice');
      if (code === 'AHA3D') {
        appliedDiscount = 0.20; // 20%
        notice.classList.remove('hidden', 'text-red-400');
        notice.classList.add('text-emerald-400');
        notice.innerText = "✓ 20% AHA3D Chef Discount Applied!";
        playUiSound(920);
        showToast("Coupon AHA3D Applied: 20% OFF! 🎉");
      } else {
        appliedDiscount = 0;
        notice.classList.remove('hidden', 'text-emerald-400');
        notice.classList.add('text-red-400');
        notice.innerText = "Invalid Promo Code. Try: AHA3D";
        playUiSound(300);
      }
      updateCartUI();
    }

    function openCheckoutModal() {
      if (cart.length === 0) {
        showToast("Your cart is empty! Add items first. 🛒");
        playUiSound(320);
        return;
      }
      toggleCartDrawer(false);
      document.getElementById('checkoutModal').classList.remove('hidden');
      playUiSound(720);
    }

    function closeCheckoutModal() {
      document.getElementById('checkoutModal').classList.add('hidden');
      cart = [];
      appliedDiscount = 0;
      document.getElementById('promoNotice').classList.add('hidden');
      document.getElementById('promoInput').value = '';
      updateCartUI();
      showToast("Order initiated! Live tracker started. 🛵");
      document.getElementById('tracking').scrollIntoView({ behavior: 'smooth' });
    }

    // Live Tracking Stage Advance Simulator
    let trackingStage = 2;
    function advanceTrackingDemo() {
      trackingStage = (trackingStage % 4) + 1;
      playUiSound(600 + trackingStage * 100);

      for (let i = 1; i <= 4; i++) {
        const circle = document.getElementById(`step-circle-${i}`);
        const col = document.getElementById(`step-col-${i}`);
        if (i < trackingStage) {
          circle.className = "w-10 h-10 mx-auto rounded-full bg-emerald-500 text-brand-charcoal font-bold flex items-center justify-center text-sm shadow-lg mb-2";
          circle.innerText = "✓";
          if (col) col.classList.remove('opacity-60');
        } else if (i === trackingStage) {
          circle.className = "w-10 h-10 mx-auto rounded-full bg-brand-saffron text-white font-bold flex items-center justify-center text-sm shadow-glow-saffron mb-2 animate-pulse";
          if (col) col.classList.remove('opacity-60');
          circle.innerText = i === 2 ? '🍳' : i === 3 ? '📦' : '🛵';
        } else {
          circle.className = "w-10 h-10 mx-auto rounded-full bg-brand-card border border-brand-border text-slate-400 font-bold flex items-center justify-center text-sm mb-2";
          circle.innerText = i === 3 ? '📦' : '🛵';
          if (col) col.classList.add('opacity-60');
        }
      }

      const stageNames = ["Received", "Sautéing & Frying", "Sterile Packaging", "Rider Out For Delivery"];
      showToast(`Tracking update: ${stageNames[trackingStage - 1]}`);
    }

    // Toast Notification helper
    function showToast(msg) {
      const container = document.getElementById('toastContainer');
      const toast = document.createElement('div');
      toast.className = "bg-brand-dark/95 border border-brand-border text-white px-4 py-2.5 rounded-xl shadow-xl text-xs flex items-center space-x-2 backdrop-blur-md transform transition-all duration-300 translate-y-4 opacity-0 pointer-events-auto";
      toast.innerHTML = `
        <span class="w-2 h-2 rounded-full bg-brand-saffron animate-ping"></span>
        <span class="font-medium">${msg}</span>
      `;
      container.appendChild(toast);

      setTimeout(() => {
        toast.classList.remove('translate-y-4', 'opacity-0');
      }, 20);

      setTimeout(() => {
        toast.classList.add('opacity-0', 'translate-y-2');
        setTimeout(() => toast.remove(), 300);
      }, 3000);
    }

    // Mobile nav drawer
    function toggleMobileNav() {
      const menu = document.getElementById('mobileMenu');
      menu.classList.toggle('hidden');
    }

    // Dynamic Live telemetry randomizer for realism
    setInterval(() => {
      const activeOrd = document.getElementById('activeOrdersVal');
      if (activeOrd) {
        const rand = 20 + Math.floor(Math.random() * 12);
        activeOrd.innerText = rand;
      }
    }, 6000);

    window.onload = function () {
      init3DScene();
      renderMenu();
      lucide.createIcons();
    };
  </script>
</body>
</html>
