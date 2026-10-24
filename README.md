# wastemanagement-project
[garbage collection.html](https://github.com/user-attachments/files/33908154/garbage.collection.html)
<!DOCTYPE html>
<!--
================================================================================
WasteLink Kenya - Modern Garbage Collection & Recycling Platform
Complete Single-Page Application / Multi-Page Prototype HTML Prompt Blueprint
================================================================================
Includes:
1. Multi-step Pickup Booking Form (Waste types, Location, Date/Time)
2. Interactive Dynamic M-Pesa Payment Simulator (STK Push simulation & Receipt)
3. Live Collector & Recycler Directory (Filterable search, ratings, coverage)
4. User Dashboard (Active Pickups, Pickup History, Automated Notifications, Eco-Stats)
5. Service Coverage Map & Calculator Visualizer
6. Collector Portal Request Overview & Analytics
7. Responsive Modern UI with Tailwind CSS, Lucide Icons, and Alpine.js State Management
-->
<html lang="en" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WasteLink Kenya | Connected Waste Management & Recycling Platform</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Alpine.js for lightweight state management -->
    <script defer src="https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0fdf4',
                            100: '#dcfce7',
                            500: '#22c55e',
                            600: '#16a34a',
                            700: '#15803d',
                            800: '#166534',
                            900: '#14532d',
                            mpesa: '#00A859'
                        }
                    },
                    fontFamily: {
                        sans: ['Plus Jakarta Sans', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        [x-cloak] { display: none !important; }
        .glass-card {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(229, 231, 235, 0.8);
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 font-sans antialiased min-h-screen flex flex-col" x-data="wasteLinkApp()">

    <!-- ================= TOP NOTIFICATION BAR ================= -->
    <div class="bg-brand-900 text-brand-100 text-xs py-2 px-4 text-center flex justify-between items-center border-b border-brand-800">
        <div class="flex items-center gap-2 max-w-7xl mx-auto w-full justify-between">
            <span class="flex items-center gap-1.5">
                <span class="relative flex h-2 w-2">
                  <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-brand-500 opacity-75"></span>
                  <span class="relative inline-flex rounded-full h-2 w-2 bg-brand-500"></span>
                </span>
                Active Service Coverage: Nairobi, Kiambu, Machakos & Kajiado Counties
            </span>
            <div class="hidden sm:flex items-center gap-4">
                <a href="#directory" @click="activeTab = 'directory'" class="hover:underline text-brand-200">For Licensed Collectors</a>
                <span>|</span>
                <a href="tel:+254700000000" class="hover:underline flex items-center gap-1"><i data-lucide="phone" class="w-3 h-3"></i> 0700 000 000</a>
            </div>
        </div>
    </div>

    <!-- ================= MAIN NAVIGATION ================= -->
    <header class="sticky top-0 z-40 bg-white/90 backdrop-blur-md border-b border-slate-200 shadow-sm">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between items-center h-20">
                <!-- Logo -->
                <div class="flex items-center gap-3 cursor-pointer" @click="activeTab = 'home'">
                    <div class="w-11 h-11 bg-gradient-to-tr from-brand-700 to-brand-500 rounded-xl flex items-center justify-center text-white shadow-md shadow-brand-500/20">
                        <i data-lucide="recycle" class="w-6 h-6 animate-spin-slow"></i>
                    </div>
                    <div>
                        <span class="text-2xl font-extrabold tracking-tight text-slate-900">Waste<span class="text-brand-600">Link</span></span>
                        <span class="block text-[10px] font-bold text-slate-400 -mt-1 tracking-widest uppercase">Kenya Digital Eco-Hub</span>
                    </div>
                </div>

                <!-- Navigation Links -->
                <nav class="hidden md:flex items-center gap-1 lg:gap-2">
                    <button @click="activeTab = 'home'" :class="activeTab === 'home' ? 'text-brand-700 bg-brand-50 font-semibold' : 'text-slate-600 hover:text-slate-900 hover:bg-slate-50'" class="px-4 py-2 rounded-lg text-sm transition">Home</button>
                    <button @click="activeTab = 'booking'" :class="activeTab === 'booking' ? 'text-brand-700 bg-brand-50 font-semibold' : 'text-slate-600 hover:text-slate-900 hover:bg-slate-50'" class="px-4 py-2 rounded-lg text-sm transition">Request Pickup</button>
                    <button @click="activeTab = 'directory'" :class="activeTab === 'directory' ? 'text-brand-700 bg-brand-50 font-semibold' : 'text-slate-600 hover:text-slate-900 hover:bg-slate-50'" class="px-4 py-2 rounded-lg text-sm transition">Find Collectors</button>
                    <button @click="activeTab = 'dashboard'" :class="activeTab === 'dashboard' ? 'text-brand-700 bg-brand-50 font-semibold' : 'text-slate-600 hover:text-slate-900 hover:bg-slate-50'" class="px-4 py-2 rounded-lg text-sm transition flex items-center gap-1.5">
                        <span>Dashboard</span>
                        <span class="bg-brand-100 text-brand-800 text-[10px] font-bold px-1.5 py-0.5 rounded-full" x-text="notifications.length"></span>
                    </button>
                </nav>

                <!-- CTA & Mobile Toggle -->
                <div class="flex items-center gap-3">
                    <button @click="activeTab = 'booking'" class="hidden sm:inline-flex items-center justify-center gap-2 bg-brand-600 hover:bg-brand-700 text-white font-medium px-5 py-2.5 rounded-xl shadow-sm transition hover:shadow-md">
                        <i data-lucide="truck" class="w-4 h-4"></i>
                        <span>Book Pickup</span>
                    </button>
                    
                    <button @click="mobileMenuOpen = !mobileMenuOpen" class="md:hidden p-2 text-slate-600 hover:text-slate-900 rounded-lg border border-slate-200">
                        <i data-lucide="menu" class="w-6 h-6"></i>
                    </button>
                </div>
            </div>
        </div>

        <!-- Mobile Dropdown -->
        <div x-show="mobileMenuOpen" x-cloak class="md:hidden border-b border-slate-200 bg-white px-4 py-3 space-y-2">
            <button @click="activeTab = 'home'; mobileMenuOpen = false" class="w-full text-left px-3 py-2 rounded-md hover:bg-slate-50 text-sm font-medium">Home</button>
            <button @click="activeTab = 'booking'; mobileMenuOpen = false" class="w-full text-left px-3 py-2 rounded-md hover:bg-slate-50 text-sm font-medium">Request Pickup</button>
            <button @click="activeTab = 'directory'; mobileMenuOpen = false" class="w-full text-left px-3 py-2 rounded-md hover:bg-slate-50 text-sm font-medium">Find Collectors</button>
            <button @click="activeTab = 'dashboard'; mobileMenuOpen = false" class="w-full text-left px-3 py-2 rounded-md hover:bg-slate-50 text-sm font-medium">My Dashboard</button>
        </div>
    </header>

    <!-- ================= MAIN CONTENT WRAPPER ================= -->
    <main class="flex-grow">

        <!-- ================= PAGE 1: HOME LANDING PAGE ================= -->
        <div x-show="activeTab === 'home'" class="space-y-16 pb-16">
            <!-- Hero Section -->
            <section class="relative overflow-hidden bg-gradient-to-b from-brand-50/50 via-white to-slate-50 pt-12 pb-20 lg:pt-20 lg:pb-28">
                <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative z-10">
                    <div class="grid lg:grid-cols-12 gap-12 items-center">
                        <div class="lg:col-span-7 space-y-6 text-center lg:text-left">
                            <div class="inline-flex items-center gap-2 bg-brand-100/80 border border-brand-200 text-brand-800 px-3.5 py-1.5 rounded-full text-xs font-semibold tracking-wide uppercase">
                                <i data-lucide="shield-check" class="w-4 h-4 text-brand-600"></i> NEMA & County Government Licensed
                            </div>
                            <h1 class="text-4xl sm:text-5xl lg:text-6xl font-extrabold text-slate-900 tracking-tight leading-none">
                                Smart Waste Pickup & Recycling <span class="text-brand-600">At Your Doorstep</span>
                            </h1>
                            <p class="text-lg text-slate-600 max-w-2xl mx-auto lg:mx-0 font-normal leading-relaxed">
                                Seamlessly connect households & commercial properties with verified, licensed private garbage collectors. Request pickups, sort recyclables, pay via M-Pesa, and get instant SMS & push updates.
                            </p>

                            <!-- User Type Selector Switcher -->
                            <div class="flex flex-col sm:flex-row gap-4 justify-center lg:justify-start pt-2">
                                <button @click="activeTab = 'booking'; userType = 'household'" class="bg-brand-600 hover:bg-brand-700 text-white font-semibold px-7 py-3.5 rounded-xl shadow-lg shadow-brand-600/30 transition flex items-center justify-center gap-2">
                                    <i data-lucide="home" class="w-5 h-5"></i>
                                    <span>Household Pickup</span>
                                </button>
                                <button @click="activeTab = 'booking'; userType = 'commercial'" class="bg-slate-900 hover:bg-slate-800 text-white font-semibold px-7 py-3.5 rounded-xl shadow-md transition flex items-center justify-center gap-2">
                                    <i data-lucide="building-2" class="w-5 h-5"></i>
                                    <span>Commercial & Estates</span>
                                </button>
                            </div>

                            <!-- Trust Indicators -->
                            <div class="pt-8 border-t border-slate-200/80 grid grid-cols-3 gap-4 text-left max-w-lg mx-auto lg:mx-0">
                                <div>
                                    <span class="block text-2xl font-bold text-slate-900">100%</span>
                                    <span class="text-xs text-slate-500">Licensed Collectors</span>
                                </div>
                                <div>
                                    <span class="block text-2xl font-bold text-slate-900">Instant</span>
                                    <span class="text-xs text-slate-500">M-Pesa Express STK</span>
                                </div>
                                <div>
                                    <span class="block text-2xl font-bold text-slate-900">24/7</span>
                                    <span class="text-xs text-slate-500">Automated SMS Alerts</span>
                                </div>
                            </div>
                        </div>

                        <!-- Hero Interactive Visual Card -->
                        <div class="lg:col-span-5">
                            <div class="bg-white rounded-2xl shadow-xl border border-slate-100 p-6 space-y-5 relative">
                                <div class="absolute -top-3 -right-3 bg-brand-500 text-white text-xs font-bold px-3 py-1 rounded-full shadow-md">
                                    LIVE STATUS
                                </div>
                                <div class="flex items-center gap-3 border-b border-slate-100 pb-4">
                                    <div class="w-10 h-10 rounded-full bg-brand-100 text-brand-700 flex items-center justify-center font-bold">
                                        <i data-lucide="map-pin" class="w-5 h-5"></i>
                                    </div>
                                    <div>
                                        <h4 class="font-semibold text-slate-800 text-sm">Kilimani & Westlands Sector</h4>
                                        <p class="text-xs text-slate-500">Truck #4 En Route | 3 Collection Slots Left Today</p>
                                    </div>
                                </div>

                                <!-- Mini Calculator / Quick Estimate -->
                                <div class="space-y-3 bg-slate-50 p-4 rounded-xl">
                                    <span class="text-xs font-bold text-slate-500 tracking-wider uppercase">Quick Price Estimator</span>
                                    <div class="grid grid-cols-2 gap-2 text-xs">
                                        <div class="bg-white p-2.5 rounded-lg border border-slate-200">
                                            <span class="block text-slate-400">Standard Household</span>
                                            <strong class="text-slate-800 text-sm">KES 20 / bag</strong>
                                        </div>
                                        <div class="bg-white p-2.5 rounded-lg border border-slate-200">
                                            <span class="block text-slate-400">Sorted Recyclables</span>
                                            <strong class="text-brand-600 text-sm">KES 20 / bag</strong>
                                        </div>
                                    </div>
                                    <button @click="activeTab = 'booking'" class="w-full text-center text-xs font-bold text-brand-700 hover:text-brand-800 py-1">Calculate exact bulk price &rarr;</button>
                                </div>

                                <!-- Sample Notification simulation -->
                                <div class="bg-brand-50 border border-brand-200 rounded-xl p-3 flex items-start gap-3">
                                    <div class="p-1.5 bg-brand-600 text-white rounded-lg">
                                        <i data-lucide="bell" class="w-4 h-4"></i>
                                    </div>
                                    <div class="text-xs">
                                        <p class="font-bold text-brand-900">SMS Notification Alert</p>
                                        <p class="text-brand-700">"Your pickup request #WL-8921 has been accepted by CleanCity Logistics. Truck arriving at 02:30 PM."</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <!-- How It Works Section -->
            <section class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="text-center max-w-3xl mx-auto space-y-3 mb-12">
                    <h2 class="text-xs font-bold text-brand-600 tracking-widest uppercase">Simple 4-Step Process</h2>
                    <h3 class="text-3xl font-extrabold text-slate-900">How WasteLink Connects You</h3>
                    <p class="text-slate-600 text-sm">We bridge the gap between waste generators and registered collectors to build cleaner, greener Kenyan towns.</p>
                </div>

                <div class="grid md:grid-cols-4 gap-8">
                    <!-- Step 1 -->
                    <div class="bg-white p-6 rounded-2xl border border-slate-100 shadow-sm hover:shadow-md transition text-center space-y-4">
                        <div class="w-14 h-14 bg-brand-100 text-brand-700 rounded-2xl flex items-center justify-center mx-auto text-xl font-bold">1</div>
                        <h4 class="font-bold text-slate-900">Select Waste Type</h4>
                        <p class="text-xs text-slate-500 leading-relaxed">Choose standard organic municipal waste, sorted plastic, electronic scrap, glass, or bulk construction debris.</p>
                    </div>
                    <!-- Step 2 -->
                    <div class="bg-white p-6 rounded-2xl border border-slate-100 shadow-sm hover:shadow-md transition text-center space-y-4">
                        <div class="w-14 h-14 bg-brand-100 text-brand-700 rounded-2xl flex items-center justify-center mx-auto text-xl font-bold">2</div>
                        <h4 class="font-bold text-slate-900">Choose Licensed Collector</h4>
                        <p class="text-xs text-slate-500 leading-relaxed">Select from NEMA-approved private waste operators covering your specific estate or zone.</p>
                    </div>
                    <!-- Step 3 -->
                    <div class="bg-white p-6 rounded-2xl border border-slate-100 shadow-sm hover:shadow-md transition text-center space-y-4">
                        <div class="w-14 h-14 bg-brand-100 text-brand-700 rounded-2xl flex items-center justify-center mx-auto text-xl font-bold">3</div>
                        <h4 class="font-bold text-slate-900">Instant M-Pesa Payment</h4>
                        <p class="text-xs text-slate-500 leading-relaxed">Receive an automated M-Pesa STK push prompt on your mobile phone to complete transaction securely.</p>
                    </div>
                    <!-- Step 4 -->
                    <div class="bg-white p-6 rounded-2xl border border-slate-100 shadow-sm hover:shadow-md transition text-center space-y-4">
                        <div class="w-14 h-14 bg-brand-100 text-brand-700 rounded-2xl flex items-center justify-center mx-auto text-xl font-bold">4</div>
                        <h4 class="font-bold text-slate-900">Track & Schedule</h4>
                        <p class="text-xs text-slate-500 leading-relaxed">Receive instant SMS scheduling alerts, track truck arrival, and view your eco-recycling metrics.</p>
                    </div>
                </div>
            </section>
        </div>

        <!-- ================= PAGE 2: REQUEST PICKUP & MPESA WORKFLOW ================= -->
        <div x-show="activeTab === 'booking'" class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 py-10" x-cloak>
            <div class="bg-white rounded-2xl shadow-xl border border-slate-200/80 overflow-hidden">
                <!-- Stepper Progress Header -->
                <div class="bg-slate-900 p-6 text-white">
                    <div class="flex items-center justify-between mb-4">
                        <div>
                            <h2 class="text-2xl font-bold">Schedule Garbage Pickup</h2>
                            <p class="text-xs text-slate-400">Fill in details to match with licensed collectors and process payment</p>
                        </div>
                        <span class="bg-brand-600 text-white text-xs font-bold px-3 py-1 rounded-full" x-text="`Step ${bookingStep} of 3`"></span>
                    </div>

                    <!-- Step Indicators -->
                    <div class="grid grid-cols-3 gap-2 text-center text-xs pt-2">
                        <div :class="bookingStep >= 1 ? 'border-brand-500 text-brand-400 font-bold' : 'border-slate-700 text-slate-500'" class="border-b-2 pb-2">1. Waste & Location</div>
                        <div :class="bookingStep >= 2 ? 'border-brand-500 text-brand-400 font-bold' : 'border-slate-700 text-slate-500'" class="border-b-2 pb-2">2. Select Collector</div>
                        <div :class="bookingStep >= 3 ? 'border-brand-500 text-brand-400 font-bold' : 'border-slate-700 text-slate-500'" class="border-b-2 pb-2">3. M-Pesa Checkout</div>
                    </div>
                </div>

                <div class="p-6 md:p-8">

                    <!-- STEP 1: WASTE TYPES & LOCATION DETAILS -->
                    <div x-show="bookingStep === 1" class="space-y-6">
                        <h3 class="text-lg font-bold text-slate-900 border-b pb-2 flex items-center gap-2">
                            <i data-lucide="trash-2" class="w-5 h-5 text-brand-600"></i> Select Waste Types & Quantity
                        </h3>

                        <!-- Category Grid -->
                        <div class="grid sm:grid-cols-3 gap-4">
                            <!-- Organic / Household -->
                            <label :class="bookingForm.wasteType === 'household' ? 'ring-2 ring-brand-600 bg-brand-50/50' : 'bg-slate-50 hover:bg-slate-100'" class="border rounded-xl p-4 cursor-pointer transition flex flex-col justify-between">
                                <div class="flex justify-between items-start mb-2">
                                    <input type="radio" name="wasteType" value="household" x-model="bookingForm.wasteType" class="text-brand-600 focus:ring-brand-500">
                                    <i data-lucide="apple" class="w-5 h-5 text-emerald-600"></i>
                                </div>
                                <div>
                                    <span class="block font-bold text-slate-900 text-sm">General Household</span>
                                    <span class="text-[11px] text-slate-500">Food waste, standard residential refuse</span>
                                </div>
                                <span class="text-xs font-bold text-brand-700 mt-2 block">KES 20 / bag</span>
                            </label>

                            <!-- Recyclables -->
                            <label :class="bookingForm.wasteType === 'recyclables' ? 'ring-2 ring-brand-600 bg-brand-50/50' : 'bg-slate-50 hover:bg-slate-100'" class="border rounded-xl p-4 cursor-pointer transition flex flex-col justify-between">
                                <div class="flex justify-between items-start mb-2">
                                    <input type="radio" name="wasteType" value="recyclables" x-model="bookingForm.wasteType" class="text-brand-600 focus:ring-brand-500">
                                    <i data-lucide="boxes" class="w-5 h-5 text-blue-600"></i>
                                </div>
                                <div>
                                    <span class="block font-bold text-slate-900 text-sm">Sorted Recyclables</span>
                                    <span class="text-[11px] text-slate-500">Plastics, PET bottles, Paper, Cardboard</span>
                                </div>
                                <span class="text-xs font-bold text-brand-700 mt-2 block">KES 20 / bag</span>
                            </label>

                            <!-- E-Waste / Hazardous -->
                            <label :class="bookingForm.wasteType === 'ewaste' ? 'ring-2 ring-brand-600 bg-brand-50/50' : 'bg-slate-50 hover:bg-slate-100'" class="border rounded-xl p-4 cursor-pointer transition flex flex-col justify-between">
                                <div class="flex justify-between items-start mb-2">
                                    <input type="radio" name="wasteType" value="ewaste" x-model="bookingForm.wasteType" class="text-brand-600 focus:ring-brand-500">
                                    <i data-lucide="cpu" class="w-5 h-5 text-amber-600"></i>
                                </div>
                                <div>
                                    <span class="block font-bold text-slate-900 text-sm">E-Waste & Electronics</span>
                                    <span class="text-[11px] text-slate-500">Old appliances, cables, batteries</span>
                                </div>
                                <span class="text-xs font-bold text-brand-700 mt-2 block">KES 20 / item</span>
                            </label>
                        </div>

                        <!-- Quantity Selector -->
                        <div class="grid sm:grid-cols-2 gap-6 bg-slate-50 p-4 rounded-xl border border-slate-200">
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-1">Quantity (Bags / Units)</label>
                                <div class="flex items-center gap-3">
                                    <button @click="if(bookingForm.bags > 1) bookingForm.bags--" class="w-10 h-10 bg-white border border-slate-300 rounded-lg font-bold text-slate-700 hover:bg-slate-100">-</button>
                                    <span class="text-lg font-bold text-slate-900 w-8 text-center" x-text="bookingForm.bags"></span>
                                    <button @click="bookingForm.bags++" class="w-10 h-10 bg-white border border-slate-300 rounded-lg font-bold text-slate-700 hover:bg-slate-100">+</button>
                                </div>
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-1">Preferred Collection Date</label>
                                <input type="date" x-model="bookingForm.date" class="w-full bg-white border border-slate-300 rounded-lg px-3 py-2 text-sm focus:ring-2 focus:ring-brand-500 outline-none">
                            </div>
                        </div>

                        <!-- Address Form -->
                        <div class="space-y-4">
                            <h4 class="text-sm font-bold text-slate-900">Pickup Location Details</h4>
                            <div class="grid sm:grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-xs text-slate-600 mb-1">Sub-County / Area</label>
                                    <select x-model="bookingForm.location" class="w-full bg-slate-50 border border-slate-300 rounded-lg px-3 py-2 text-sm focus:ring-2 focus:ring-brand-500 outline-none">
                                        <option value="Westlands">Westlands / Parklands</option>
                                        <option value="Kilimani">Kilimani / Lavington</option>
                                        <option value="Kasarani">Kasarani / Roysambu</option>
                                        <option value="Langata">Lang'ata / Karen</option>
                                        <option value="Ruaka">Ruaka / Kiambu Rd</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block text-xs text-slate-600 mb-1">Estate / Apartment Name & Door No.</label>
                                    <input type="text" x-model="bookingForm.address" placeholder="e.g. Sunrise Estate, House B4" class="w-full bg-slate-50 border border-slate-300 rounded-lg px-3 py-2 text-sm focus:ring-2 focus:ring-brand-500 outline-none">
                                </div>
                            </div>
                        </div>

                        <div class="flex justify-end pt-4">
                            <button @click="bookingStep = 2" class="bg-brand-600 hover:bg-brand-700 text-white font-semibold px-6 py-2.5 rounded-xl shadow-md flex items-center gap-2">
                                <span>Proceed to Collector Selection</span>
                                <i data-lucide="arrow-right" class="w-4 h-4"></i>
                            </button>
                        </div>
                    </div>

                    <!-- STEP 2: SELECT LICENSED COLLECTOR -->
                    <div x-show="bookingStep === 2" class="space-y-6" x-cloak>
                        <h3 class="text-lg font-bold text-slate-900 border-b pb-2 flex items-center gap-2">
                            <i data-lucide="truck" class="w-5 h-5 text-brand-600"></i> Choose Available Licensed Collectors in <span class="text-brand-600" x-text="bookingForm.location"></span>
                        </h3>

                        <div class="space-y-3">
                            <template x-for="collector in collectors" :key="collector.id">
                                <label :class="bookingForm.collectorId === collector.id ? 'border-brand-600 bg-brand-50/40 ring-2 ring-brand-500' : 'border-slate-200 bg-white hover:border-slate-300'" class="border rounded-2xl p-4 flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 cursor-pointer transition">
                                    <div class="flex items-center gap-3">
                                        <input type="radio" name="selectedCollector" :value="collector.id" x-model="bookingForm.collectorId" class="text-brand-600 focus:ring-brand-500">
                                        <div>
                                            <div class="flex items-center gap-2">
                                                <h4 class="font-bold text-slate-900 text-sm" x-text="collector.name"></h4>
                                                <span class="bg-emerald-100 text-emerald-800 text-[10px] font-bold px-2 py-0.5 rounded-full" x-text="collector.license"></span>
                                            </div>
                                            <p class="text-xs text-slate-500 mt-0.5" x-text="`Rating: ${collector.rating} ★ (${collector.reviews} reviews) • Servicing ${collector.zone}`"></p>
                                        </div>
                                    </div>
                                    <div class="text-right w-full sm:w-auto flex sm:flex-col justify-between items-center sm:items-end border-t sm:border-0 pt-2 sm:pt-0">
                                        <span class="text-xs text-slate-500">Estimated Pickup</span>
                                        <span class="font-bold text-slate-800 text-sm" x-text="collector.nextSlot"></span>
                                    </div>
                                </label>
                            </template>
                        </div>

                        <div class="flex justify-between pt-4">
                            <button @click="bookingStep = 1" class="border border-slate-300 text-slate-700 hover:bg-slate-50 font-semibold px-5 py-2.5 rounded-xl">Back</button>
                            <button @click="bookingStep = 3" class="bg-brand-600 hover:bg-brand-700 text-white font-semibold px-6 py-2.5 rounded-xl shadow-md flex items-center gap-2">
                                <span>Continue to Payment</span>
                                <i data-lucide="arrow-right" class="w-4 h-4"></i>
                            </button>
                        </div>
                    </div>

                    <!-- STEP 3: MPESA EXPRESS CHECKOUT & STK SIMULATOR -->
                    <div x-show="bookingStep === 3" class="space-y-6" x-cloak>
                        <h3 class="text-lg font-bold text-slate-900 border-b pb-2 flex items-center gap-2">
                            <i data-lucide="smartphone" class="w-5 h-5 text-brand-mpesa"></i> Pay via Safaricom M-Pesa
                        </h3>

                        <div class="grid md:grid-cols-2 gap-6">
                            <!-- Order Summary -->
                            <div class="bg-slate-50 p-5 rounded-2xl border border-slate-200 space-y-3">
                                <h4 class="text-xs font-bold text-slate-400 uppercase tracking-wider">Pickup Order Summary</h4>
                                <div class="space-y-2 text-xs">
                                    <div class="flex justify-between text-slate-600">
                                        <span>Waste Category:</span>
                                        <strong class="text-slate-900 capitalize" x-text="bookingForm.wasteType"></strong>
                                    </div>
                                    <div class="flex justify-between text-slate-600">
                                        <span>Quantity:</span>
                                        <strong class="text-slate-900" x-text="`${bookingForm.bags} Bag(s)`"></strong>
                                    </div>
                                    <div class="flex justify-between text-slate-600">
                                        <span>Location:</span>
                                        <strong class="text-slate-900" x-text="`${bookingForm.address}, ${bookingForm.location}`"></strong>
                                    </div>
                                    <div class="flex justify-between text-slate-600">
                                        <span>Assigned Collector:</span>
                                        <strong class="text-slate-900" x-text="getSelectedCollector()?.name"></strong>
                                    </div>
                                    <div class="border-t border-slate-200 pt-2 flex justify-between text-sm font-bold">
                                        <span class="text-slate-800">Total Payable Amount:</span>
                                        <span class="text-brand-700 text-base" x-text="`KES ${calculateTotal()}`"></span>
                                    </div>
                                </div>
                            </div>

                            <!-- M-Pesa Interactive STK Simulation Form -->
                            <div class="space-y-4">
                                <div class="bg-brand-50 border border-brand-200 p-4 rounded-xl flex items-center gap-3">
                                    <div class="w-10 h-10 rounded-lg bg-brand-mpesa text-white font-black text-xs flex items-center justify-center tracking-tighter">
                                        M-PESA
                                    </div>
                                    <div>
                                        <h5 class="text-xs font-bold text-slate-900">M-Pesa Express (STK Push)</h5>
                                        <p class="text-[11px] text-slate-600">You will receive a prompt on your phone to enter PIN.</p>
                                    </div>
                                </div>

                                <div>
                                    <label class="block text-xs font-bold text-slate-700 mb-1">M-Pesa Mobile Phone Number</label>
                                    <div class="relative">
                                        <span class="absolute left-3 top-2.5 text-xs font-bold text-slate-400">+254</span>
                                        <input type="tel" x-model="bookingForm.phone" placeholder="712345678" class="w-full bg-slate-50 border border-slate-300 rounded-lg pl-14 pr-3 py-2 text-sm focus:ring-2 focus:ring-brand-mpesa outline-none">
                                    </div>
                                </div>

                                <button @click="triggerMpesaPayment()" :disabled="paymentProcessing" class="w-full bg-brand-mpesa hover:bg-emerald-700 text-white font-bold py-3 rounded-xl shadow-md transition flex items-center justify-center gap-2">
                                    <template x-if="!paymentProcessing">
                                        <span class="flex items-center gap-2">
                                            <i data-lucide="lock" class="w-4 h-4"></i>
                                            <span x-text="`Pay KES ${calculateTotal()} via M-Pesa`"></span>
                                        </span>
                                    </template>
                                    <template x-if="paymentProcessing">
                                        <span class="flex items-center gap-2">
                                            <svg class="animate-spin h-4 w-4 text-white" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4" fill="none"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path></svg>
                                            <span>Sending STK Prompt to Phone...</span>
                                        </span>
                                    </template>
                                </button>
                            </div>
                        </div>

                        <!-- Simulated Phone STK Push Modal Overlay -->
                        <div x-show="showStkPushModal" x-cloak class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4">
                            <div class="bg-slate-900 border-2 border-slate-700 w-full max-w-xs rounded-3xl p-6 text-white text-center shadow-2xl space-y-4">
                                <div class="w-12 h-12 bg-brand-mpesa rounded-full flex items-center justify-center mx-auto">
                                    <i data-lucide="smartphone" class="w-6 h-6 text-white"></i>
                                </div>
                                <h4 class="font-bold text-sm text-emerald-400">SIMULATED M-PESA STK PUSH</h4>
                                <div class="bg-slate-800 p-3 rounded-xl text-xs space-y-1 text-slate-300">
                                    <p>Do you want to pay <strong class="text-white" x-text="`KES ${calculateTotal()}`"></strong> to <strong class="text-white">WasteLink Kenya</strong> for Garbage Collection Ref #WL-9042?</p>
                                </div>
                                <input type="password" maxlength="4" placeholder="Enter M-Pesa PIN" class="w-full text-center tracking-widest text-lg font-bold bg-slate-800 border border-slate-600 rounded-lg py-2 text-white outline-none focus:border-brand-mpesa">
                                <div class="grid grid-cols-2 gap-2 pt-2">
                                    <button @click="showStkPushModal = false; paymentProcessing = false" class="bg-slate-700 hover:bg-slate-600 text-xs py-2 rounded-lg font-bold">Cancel</button>
                                    <button @click="completePayment()" class="bg-brand-mpesa hover:bg-emerald-600 text-xs py-2 rounded-lg font-bold">Confirm PIN</button>
                                </div>
                            </div>
                        </div>

                        <div class="flex justify-start border-t pt-4">
                            <button @click="bookingStep = 2" class="border border-slate-300 text-slate-700 hover:bg-slate-50 font-semibold px-5 py-2.5 rounded-xl text-xs">Back</button>
                        </div>
                    </div>

                </div>
            </div>
        </div>

        <!-- ================= PAGE 3: LICENSED COLLECTORS & RECYCLERS DIRECTORY ================= -->
        <div x-show="activeTab === 'directory'" class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10" x-cloak>
            <div class="space-y-6">
                <div>
                    <h2 class="text-3xl font-extrabold text-slate-900">Verified Private Collectors & Recyclers</h2>
                    <p class="text-slate-600 text-sm mt-1">NEMA and County Government licensed private waste contractors across Kenya</p>
                </div>

                <!-- Filters -->
                <div class="bg-white p-4 rounded-xl border border-slate-200 shadow-sm flex flex-col md:flex-row gap-4 justify-between items-center">
                    <div class="relative w-full md:w-80">
                        <i data-lucide="search" class="w-4 h-4 text-slate-400 absolute left-3 top-3"></i>
                        <input type="text" x-model="directorySearch" placeholder="Search by name, county, or estate..." class="w-full pl-9 pr-4 py-2 bg-slate-50 border border-slate-200 rounded-lg text-xs focus:outline-none focus:ring-2 focus:ring-brand-500">
                    </div>
                    <div class="flex gap-2 w-full md:w-auto">
                        <select x-model="directoryCategory" class="bg-slate-50 border border-slate-200 rounded-lg px-3 py-2 text-xs font-medium text-slate-700 outline-none">
                            <option value="all">All Waste Types</option>
                            <option value="household">Household Municipal</option>
                            <option value="recyclables">Plastic & Paper Recyclers</option>
                            <option value="ewaste">Electronic Waste Specialists</option>
                        </select>
                    </div>
                </div>

                <!-- Collectors List Grid -->
                <div class="grid md:grid-cols-3 gap-6">
                    <template x-for="item in filteredDirectory" :key="item.id">
                        <div class="bg-white rounded-2xl border border-slate-200/80 p-6 shadow-sm hover:shadow-md transition space-y-4 flex flex-col justify-between">
                            <div class="space-y-3">
                                <div class="flex justify-between items-start">
                                    <span class="bg-brand-100 text-brand-800 text-[10px] font-bold px-2.5 py-1 rounded-full" x-text="item.type"></span>
                                    <span class="text-xs text-amber-500 font-bold flex items-center gap-1">★ <span x-text="item.rating" class="text-slate-700"></span></span>
                                </div>
                                <h3 class="font-bold text-slate-900 text-base" x-text="item.name"></h3>
                                <p class="text-xs text-slate-500" x-text="`Coverage: ${item.zone}`"></p>
                                <div class="text-[11px] text-slate-600 bg-slate-50 p-2.5 rounded-lg border border-slate-100">
                                    <span class="block font-semibold text-slate-800">NEMA License:</span>
                                    <span class="font-mono text-slate-500" x-text="item.license"></span>
                                </div>
                            </div>
                            <button @click="bookingForm.collectorId = item.id; activeTab = 'booking'; bookingStep = 1" class="w-full bg-slate-900 hover:bg-brand-600 text-white font-medium py-2 rounded-xl text-xs transition">
                                Request Pickup From This Provider
                            </button>
                        </div>
                    </template>
                </div>
            </div>
        </div>

        <!-- ================= PAGE 4: USER DASHBOARD & NOTIFICATIONS ================= -->
        <div x-show="activeTab === 'dashboard'" class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10" x-cloak>
            <div class="space-y-8">
                <!-- Dashboard Header -->
                <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 border-b border-slate-200 pb-6">
                    <div>
                        <h2 class="text-3xl font-extrabold text-slate-900">User Dashboard & Schedule</h2>
                        <p class="text-slate-600 text-sm">Monitor active pickups, payment receipts, eco-impact, and notifications</p>
                    </div>
                    <div class="flex items-center gap-3">
                        <span class="text-xs bg-brand-100 text-brand-800 font-bold px-3 py-1.5 rounded-full flex items-center gap-1">
                            <i data-lucide="leaf" class="w-3.5 h-3.5"></i> Eco Tier: Green Champion
                        </span>
                    </div>
                </div>

                <!-- Eco Impact Stats -->
                <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                    <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm">
                        <span class="text-xs text-slate-500 block font-medium">Total Pickups Done</span>
                        <span class="text-2xl font-bold text-slate-900 mt-1 block">14</span>
                    </div>
                    <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm">
                        <span class="text-xs text-slate-500 block font-medium">Recyclables Diverted</span>
                        <span class="text-2xl font-bold text-brand-600 mt-1 block">128 kg</span>
                    </div>
                    <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm">
                        <span class="text-xs text-slate-500 block font-medium">Carbon Footprint Saved</span>
                        <span class="text-2xl font-bold text-emerald-600 mt-1 block">42 kg CO₂</span>
                    </div>
                    <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm">
                        <span class="text-xs text-slate-500 block font-medium">M-Pesa Spent</span>
                        <span class="text-2xl font-bold text-slate-900 mt-1 block">KES 280</span>
                    </div>
                </div>

                <div class="grid lg:grid-cols-12 gap-8">
                    <!-- Left: Active Scheduled Pickups -->
                    <div class="lg:col-span-7 space-y-4">
                        <h3 class="text-lg font-bold text-slate-900 flex items-center gap-2">
                            <i data-lucide="calendar" class="w-5 h-5 text-brand-600"></i> Active & Recent Scheduled Pickups
                        </h3>

                        <div class="space-y-3">
                            <template x-for="item in activePickups" :key="item.id">
                                <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm space-y-3">
                                    <div class="flex justify-between items-start">
                                        <div>
                                            <span class="text-xs font-mono font-bold text-slate-400" x-text="item.id"></span>
                                            <h4 class="font-bold text-slate-900 text-sm" x-text="item.collector"></h4>
                                        </div>
                                        <span :class="{
                                            'bg-amber-100 text-amber-800': item.status === 'Scheduled',
                                            'bg-emerald-100 text-emerald-800': item.status === 'Completed',
                                            'bg-blue-100 text-blue-800': item.status === 'En Route'
                                        }" class="text-[10px] font-bold px-2.5 py-1 rounded-full" x-text="item.status"></span>
                                    </div>
                                    <div class="grid grid-cols-2 gap-2 text-xs text-slate-600 bg-slate-50 p-3 rounded-xl">
                                        <div><strong>Date/Time:</strong> <span x-text="item.date"></span></div>
                                        <div><strong>Type:</strong> <span x-text="item.type"></span></div>
                                        <div><strong>M-Pesa Ref:</strong> <span class="font-mono" x-text="item.mpesaRef"></span></div>
                                        <div><strong>Amount:</strong> <span x-text="`KES ${item.amount}`"></span></div>
                                    </div>
                                </div>
                            </template>
                        </div>
                    </div>

                    <!-- Right: Automated Schedule Notifications -->
                    <div class="lg:col-span-5 space-y-4">
                        <h3 class="text-lg font-bold text-slate-900 flex items-center gap-2">
                            <i data-lucide="bell" class="w-5 h-5 text-brand-600"></i> Live Notifications & SMS Log
                        </h3>

                        <div class="bg-white rounded-2xl border border-slate-200 shadow-sm p-4 divide-y divide-slate-100">
                            <template x-for="notif in notifications" :key="notif.id">
                                <div class="py-3 space-y-1">
                                    <div class="flex justify-between items-center text-[10px]">
                                        <span class="font-bold text-brand-600 uppercase" x-text="notif.type"></span>
                                        <span class="text-slate-400" x-text="notif.time"></span>
                                    </div>
                                    <p class="text-xs text-slate-800 font-medium" x-text="notif.title"></p>
                                    <p class="text-[11px] text-slate-500 leading-relaxed" x-text="notif.message"></p>
                                </div>
                            </template>
                        </div>
                    </div>
                </div>
            </div>
        </div>

    </main>

    <!-- ================= FOOTER ================= -->
    <footer class="bg-slate-900 text-slate-400 border-t border-slate-800 py-12">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 grid md:grid-cols-4 gap-8">
            <div class="space-y-4">
                <div class="flex items-center gap-2 text-white">
                    <i data-lucide="recycle" class="w-6 h-6 text-brand-500"></i>
                    <span class="text-xl font-bold">WasteLink Kenya</span>
                </div>
                <p class="text-xs text-slate-400 leading-relaxed">Connecting Kenyan households and enterprises with NEMA-certified collectors for efficient garbage removal and sustainable recycling.</p>
            </div>
            <div>
                <h4 class="text-white text-xs font-bold uppercase tracking-wider mb-3">Quick Navigation</h4>
                <ul class="space-y-2 text-xs">
                    <li><button @click="activeTab = 'home'" class="hover:text-white">Home</button></li>
                    <li><button @click="activeTab = 'booking'" class="hover:text-white">Request Garbage Pickup</button></li>
                    <li><button @click="activeTab = 'directory'" class="hover:text-white">Collector Directory</button></li>
                    <li><button @click="activeTab = 'dashboard'" class="hover:text-white">User Portal & SMS Log</button></li>
                </ul>
            </div>
            <div>
                <h4 class="text-white text-xs font-bold uppercase tracking-wider mb-3">Compliance & Licensing</h4>
                <p class="text-xs text-slate-400 leading-relaxed">Integrated with NEMA (National Environment Management Authority) registered private collectors across Nairobi Metropolitan area.</p>
            </div>
            <div>
                <h4 class="text-white text-xs font-bold uppercase tracking-wider mb-3">Support & Helpline</h4>
                <p class="text-xs text-slate-400">Emergency Pickup Desk:</p>
                <span class="text-sm font-bold text-white block mt-1">+254 (0) 700 000 000</span>
                <span class="text-xs text-slate-500 block mt-1">support@wastelink.co.ke</span>
            </div>
        </div>
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 mt-8 pt-6 border-t border-slate-800 text-center text-xs text-slate-500">
            &copy; 2026 WasteLink Kenya Digital Eco-Hub. All Rights Reserved.
        </div>
    </footer>

    <!-- ================= ALPINE.JS STATE MANAGEMENT ================= -->
    <script>
        document.addEventListener('alpine:init', () => {
            Alpine.data('wasteLinkApp', () => ({
                activeTab: 'home',
                mobileMenuOpen: false,
                userType: 'household',
                bookingStep: 1,
                paymentProcessing: false,
                showStkPushModal: false,
                directorySearch: '',
                directoryCategory: 'all',

                bookingForm: {
                    wasteType: 'household',
                    bags: 2,
                    date: new Date().toISOString().split('T')[0],
                    location: 'Kilimani',
                    address: '',
                    collectorId: 1,
                    phone: ''
                },

                collectors: [
                    { id: 1, name: 'CleanCity Logistics Kenya', license: 'NEMA/WM/2025/089', rating: '4.9', reviews: 142, zone: 'Kilimani, Lavington, Kileleshwa', nextSlot: 'Today, 02:30 PM' },
                    { id: 2, name: 'EcoGreen Recyclers Ltd', license: 'NEMA/WM/2024/311', rating: '4.8', reviews: 98, zone: 'Westlands, Parklands, Spring Valley', nextSlot: 'Tomorrow, 09:00 AM' },
                    { id: 3, name: 'Nairobi Bins Services', license: 'NEMA/WM/2025/112', rating: '4.7', reviews: 210, zone: 'Kasarani, Roysambu, Kahawa', nextSlot: 'Today, 04:00 PM' }
                ],

                activePickups: [
                    { id: '#WL-9042', collector: 'CleanCity Logistics Kenya', date: 'Today, 02:30 PM', type: 'Household Waste (2 Bags)', amount: '40', status: 'En Route', mpesaRef: 'QK8912LX01' },
                    { id: '#WL-8810', collector: 'EcoGreen Recyclers Ltd', date: '18 Oct 2026', type: 'Sorted Plastics (3 Bags)', amount: '60', status: 'Completed', mpesaRef: 'QK7102MM99' }
                ],

                notifications: [
                    { id: 1, type: 'SMS Alert', time: '10 mins ago', title: 'Pickup En Route', message: 'Truck #4 from CleanCity Logistics is 15 minutes away from your location (Kilimani).' },
                    { id: 2, type: 'M-Pesa Receipt', time: '1 hour ago', title: 'Payment Confirmed', message: 'Received KES 40.00 for WasteLink Booking Ref #WL-9042. M-Pesa Code: QK8912LX01.' },
                    { id: 3, type: 'System Update', time: 'Yesterday', title: 'Schedule Reminder', message: 'You have a scheduled recyclable waste pickup set for tomorrow at 09:00 AM.' }
                ],

                get filteredDirectory() {
                    return this.collectors.filter(c => {
                        const matchesSearch = c.name.toLowerCase().includes(this.directorySearch.toLowerCase()) || c.zone.toLowerCase().includes(this.directorySearch.toLowerCase());
                        return matchesSearch;
                    });
                },

                getSelectedCollector() {
                    return this.collectors.find(c => c.id === parseInt(this.bookingForm.collectorId)) || this.collectors[0];
                },

                calculateTotal() {
                    let rate = 20;
                    return rate * this.bookingForm.bags;
                },

                triggerMpesaPayment() {
                    if (!this.bookingForm.phone) {
                        alert('Please enter a valid M-Pesa phone number.');
                        return;
                    }
                    this.paymentProcessing = true;
                    setTimeout(() => {
                        this.showStkPushModal = true;
                    }, 1200);
                },

                completePayment() {
                    this.showStkPushModal = false;
                    this.paymentProcessing = false;
                    
                    const newRef = 'QK' + Math.floor(10000000 + Math.random() * 90000000);
                    const newPickup = {
                        id: '#WL-' + Math.floor(1000 + Math.random() * 9000),
                        collector: this.getSelectedCollector().name,
                        date: 'Scheduled: ' + this.bookingForm.date,
                        type: `${this.bookingForm.wasteType} (${this.bookingForm.bags} unit/bag)`,
                        amount: this.calculateTotal(),
                        status: 'Scheduled',
                        mpesaRef: newRef
                    };

                    this.activePickups.unshift(newPickup);
                    this.notifications.unshift({
                        id: Date.now(),
                        type: 'SMS Alert',
                        time: 'Just now',
                        title: 'Pickup Scheduled Successfully',
                        message: `Payment KES ${this.calculateTotal()} received (${newRef}). ${this.getSelectedCollector().name} assigned.`
                    });

                    alert(`M-Pesa Payment Successful! Ref: ${newRef}. Your pickup is scheduled.`);
                    this.activeTab = 'dashboard';
                    this.bookingStep = 1;
                }
            }));
        });
    </script>
    <script>
        // Re-initialize icons after Alpine renders DOM changes
        document.addEventListener('DOMContentLoaded', () => {
            lucide.createIcons();
        });
    </script>
</body>
</html>
