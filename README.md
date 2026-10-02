# Yabsera
Auto site
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AutoParts Hub | Premium Automotive Parts</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: { sans: ['Inter', 'sans-serif'] },
                    colors: {
                        brand: {
                            dark: '#0f172a', // slate-900
                            darker: '#020617', // slate-950
                            light: '#f8fafc', // slate-50
                            accent: '#dc2626', // red-600
                            accentHover: '#b91c1c', // red-700
                        }
                    }
                }
            }
        }
    </script>
    <style>
        /* Custom Styles for utility classes not in Tailwind or specific behaviors */
        body { -webkit-tap-highlight-color: transparent; }
        .view-section { display: none; opacity: 0; transition: opacity 0.3s ease-in-out; }
        .view-section.active { display: block; opacity: 1; }
        .scrollbar-hide::-webkit-scrollbar { display: none; }
        .scrollbar-hide { -ms-overflow-style: none; scrollbar-width: none; }
        .toast { transform: translateY(100%); transition: transform 0.3s ease-in-out; }
        .toast.show { transform: translateY(0); }
        .range-slider { -webkit-appearance: none; width: 100%; height: 4px; border-radius: 2px; background: #cbd5e1; outline: none; }
        .range-slider::-webkit-slider-thumb { -webkit-appearance: none; appearance: none; width: 16px; height: 16px; border-radius: 50%; background: #dc2626; cursor: pointer; }
    </style>
</head>
<body class="bg-brand-light text-slate-800 flex flex-col min-h-screen">

    <!-- Toast Notification -->
    <div id="toast" class="toast fixed bottom-4 right-4 z-50 bg-slate-900 text-white px-6 py-3 rounded-lg shadow-xl flex items-center gap-3">
        <i id="toast-icon" class="fas fa-check-circle text-green-400"></i>
        <span id="toast-msg">Message goes here</span>
    </div>

    <!-- Header -->
    <header class="bg-brand-dark text-white sticky top-0 z-40 shadow-md">
        <!-- Top bar -->
        <div class="hidden md:flex justify-between items-center px-6 py-1 bg-brand-darker text-xs text-slate-300">
            <div><i class="fas fa-truck mr-2"></i>Free shipping on orders over $100</div>
            <div class="flex gap-4">
                <a href="#" onclick="navigate('contact')" class="hover:text-white transition"><i class="fas fa-headset mr-1"></i>Support</a>
                <a href="#" onclick="navigate('contact')" class="hover:text-white transition"><i class="fas fa-map-marker-alt mr-1"></i>Store Locator</a>
            </div>
        </div>
        <!-- Main Nav -->
        <div class="container mx-auto px-4 py-3 sm:py-4 flex flex-wrap lg:flex-nowrap justify-between items-center gap-4">
            <!-- Logo -->
            <div class="flex items-center gap-2 cursor-pointer" onclick="navigate('home')">
                <div class="bg-brand-accent text-white p-2 rounded-lg flex items-center justify-center">
                    <i class="fas fa-car-side text-xl"></i>
                </div>
                <div>
                    <h1 class="text-xl font-bold leading-none tracking-tight">AutoParts<span class="text-brand-accent">Hub</span></h1>
                    <span class="text-[10px] text-slate-400 uppercase tracking-widest hidden sm:block">Premium Quality</span>
                </div>
            </div>

            <!-- Search Bar (Desktop) -->
            <div class="hidden lg:flex flex-1 max-w-2xl relative">
                <input type="text" id="global-search" placeholder="Search by part name, category, or vehicle..." class="w-full bg-slate-800 text-white border border-slate-700 rounded-l-lg px-4 py-2.5 focus:outline-none focus:border-brand-accent transition">
                <button onclick="performSearch()" class="bg-brand-accent hover:bg-brand-accentHover text-white px-6 py-2.5 rounded-r-lg transition flex items-center justify-center">
                    <i class="fas fa-search"></i>
                </button>
            </div>

            <!-- Icons -->
            <div class="flex items-center gap-4 sm:gap-6">
                <!-- Mobile Search Toggle -->
                <button class="lg:hidden text-slate-300 hover:text-white" onclick="document.getElementById('mobile-search').classList.toggle('hidden')">
                    <i class="fas fa-search text-lg"></i>
                </button>

                <button onclick="navigate('wishlist')" class="relative text-slate-300 hover:text-brand-accent transition group" title="Wishlist">
                    <i class="fas fa-heart text-xl group-hover:scale-110 transition-transform"></i>
                    <span id="wishlist-badge" class="absolute -top-1.5 -right-2 bg-brand-accent text-white text-[10px] font-bold h-4 w-4 rounded-full flex items-center justify-center hidden">0</span>
                </button>
                
                <button onclick="navigate('auth')" class="text-slate-300 hover:text-white transition group flex items-center gap-2" title="Account">
                    <i class="fas fa-user text-xl group-hover:scale-110 transition-transform"></i>
                    <span id="user-greeting" class="hidden sm:block text-sm font-medium">Sign In</span>
                </button>
                
                <button onclick="navigate('cart')" class="relative text-slate-300 hover:text-white transition group flex items-center gap-2" title="Cart">
                    <div class="relative">
                        <i class="fas fa-shopping-cart text-xl group-hover:scale-110 transition-transform"></i>
                        <span id="cart-badge" class="absolute -top-2 -right-2 bg-brand-accent text-white text-[10px] font-bold h-4 w-4 rounded-full flex items-center justify-center hidden">0</span>
                    </div>
                    <span id="cart-total-nav" class="hidden sm:block text-sm font-bold text-white">$0.00</span>
                </button>
            </div>
        </div>

        <!-- Mobile Search Bar (Collapsible) -->
        <div id="mobile-search" class="hidden lg:hidden px-4 pb-3 bg-brand-darker border-t border-slate-800">
            <div class="flex mt-2">
                <input type="text" id="global-search-mobile" placeholder="Search parts..." class="w-full bg-slate-800 text-white border border-slate-700 rounded-l-lg px-4 py-2 focus:outline-none focus:border-brand-accent">
                <button onclick="performSearch('mobile')" class="bg-brand-accent text-white px-4 py-2 rounded-r-lg">
                    <i class="fas fa-search"></i>
                </button>
            </div>
        </div>

        <!-- Bottom Nav Menu -->
        <div class="bg-slate-800 border-t border-slate-700 hidden md:block">
            <div class="container mx-auto px-4">
                <nav class="flex gap-8 text-sm font-medium">
                    <a href="#" onclick="navigate('home')" class="py-3 hover:text-brand-accent transition">Home</a>
                    <a href="#" onclick="navigate('shop', {cat: 'all'})" class="py-3 hover:text-brand-accent transition">All Parts</a>
                    <a href="#" onclick="navigate('shop', {cat: 'Engine'})" class="py-3 hover:text-brand-accent transition">Engine</a>
                    <a href="#" onclick="navigate('shop', {cat: 'Brakes'})" class="py-3 hover:text-brand-accent transition">Brakes</a>
                    <a href="#" onclick="navigate('shop', {cat: 'Electrical'})" class="py-3 hover:text-brand-accent transition">Electrical</a>
                    <a href="#" onclick="navigate('shop', {cat: 'Accessories'})" class="py-3 hover:text-brand-accent transition">Accessories</a>
                    <a href="#" onclick="navigate('contact')" class="py-3 hover:text-brand-accent transition ml-auto">Contact Us</a>
                </nav>
            </div>
        </div>
    </header>

    <main class="flex-grow">
        
        <!-- ================= HOME VIEW ================= -->
        <div id="view-home" class="view-section active pb-12">
            <!-- Hero -->
            <div class="relative bg-brand-dark text-white overflow-hidden">
                <div class="absolute inset-0 opacity-40 mix-blend-overlay">
                    <img src="https://images.unsplash.com/photo-1486262715619-6708c46c5905?ixlib=rb-4.0.3&auto=format&fit=crop&w=1920&q=80" alt="Car Engine" class="w-full h-full object-cover">
                </div>
                <div class="absolute inset-0 bg-gradient-to-r from-brand-dark via-brand-dark/90 to-transparent"></div>
                <div class="container mx-auto px-4 py-20 lg:py-32 relative z-10 flex flex-col items-start max-w-4xl">
                    <span class="inline-block py-1 px-3 rounded-full bg-brand-accent/20 text-brand-accent border border-brand-accent/30 text-sm font-bold tracking-wide mb-4 uppercase">Over 50,000 Parts in Stock</span>
                    <h2 class="text-4xl md:text-6xl font-bold leading-tight mb-6">Find the <span class="text-brand-accent">Right Part</span><br>For Your Vehicle</h2>
                    <p class="text-lg text-slate-300 mb-8 max-w-2xl">Premium quality automotive parts for all makes and models. Fast shipping, expert support, and guaranteed fitment.</p>
                    
                    <!-- Hero Vehicle Selector (Mock) -->
                    <div class="bg-white/10 backdrop-blur-md border border-white/20 p-4 rounded-xl w-full max-w-3xl flex flex-col sm:flex-row gap-3">
                        <select class="flex-1 bg-slate-800 text-white border-none rounded-lg px-4 py-3 focus:ring-2 focus:ring-brand-accent outline-none">
                            <option>Select Year</option>
                            <option>2024</option><option>2023</option><option>2022</option>
                        </select>
                        <select class="flex-1 bg-slate-800 text-white border-none rounded-lg px-4 py-3 focus:ring-2 focus:ring-brand-accent outline-none">
                            <option>Select Make</option>
                            <option>Toyota</option><option>Ford</option><option>Honda</option>
                        </select>
                        <select class="flex-1 bg-slate-800 text-white border-none rounded-lg px-4 py-3 focus:ring-2 focus:ring-brand-accent outline-none">
                            <option>Select Model</option>
                        </select>
                        <button onclick="navigate('shop')" class="bg-brand-accent hover:bg-brand-accentHover text-white font-bold py-3 px-8 rounded-lg transition whitespace-nowrap">
                            Find Parts
                        </button>
                    </div>
                </div>
            </div>

            <!-- Categories -->
            <div class="container mx-auto px-4 py-16">
                <div class="flex justify-between items-end mb-8">
                    <div>
                        <h3 class="text-3xl font-bold text-slate-900">Shop by Category</h3>
                        <p class="text-slate-500 mt-2">Explore our wide range of auto parts</p>
                    </div>
                    <button onclick="navigate('shop')" class="text-brand-accent font-medium hover:underline hidden sm:block">View All Categories <i class="fas fa-arrow-right ml-1"></i></button>
                </div>
                
                <div class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-7 gap-4" id="home-categories-grid">
                    <!-- Populated by JS -->
                </div>
            </div>

            <!-- Featured Products -->
            <div class="bg-white py-16 border-y border-slate-200">
                <div class="container mx-auto px-4">
                    <h3 class="text-3xl font-bold text-slate-900 mb-2 text-center">Featured Parts</h3>
                    <p class="text-slate-500 mb-10 text-center">Top rated products for your vehicle maintenance</p>
                    
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6" id="home-featured-products">
                        <!-- Populated by JS -->
                    </div>
                </div>
            </div>
            
            <!-- Features Banner -->
            <div class="container mx-auto px-4 py-16">
                <div class="grid grid-cols-1 md:grid-cols-3 gap-8 text-center">
                    <div class="p-6 rounded-2xl bg-slate-50 border border-slate-100 shadow-sm hover:shadow-md transition">
                        <div class="w-16 h-16 bg-slate-900 text-white rounded-full flex items-center justify-center mx-auto mb-4 text-2xl">
                            <i class="fas fa-shield-alt"></i>
                        </div>
                        <h4 class="text-lg font-bold mb-2">Quality Guarantee</h4>
                        <p class="text-slate-500 text-sm">All parts meet or exceed OEM specifications for reliability.</p>
                    </div>
                    <div class="p-6 rounded-2xl bg-slate-50 border border-slate-100 shadow-sm hover:shadow-md transition">
                        <div class="w-16 h-16 bg-brand-accent text-white rounded-full flex items-center justify-center mx-auto mb-4 text-2xl">
                            <i class="fas fa-truck-fast"></i>
                        </div>
                        <h4 class="text-lg font-bold mb-2">Fast Shipping</h4>
                        <p class="text-slate-500 text-sm">Same-day dispatch on orders placed before 2 PM.</p>
                    </div>
                    <div class="p-6 rounded-2xl bg-slate-50 border border-slate-100 shadow-sm hover:shadow-md transition">
                        <div class="w-16 h-16 bg-slate-900 text-white rounded-full flex items-center justify-center mx-auto mb-4 text-2xl">
                            <i class="fas fa-undo"></i>
                        </div>
                        <h4 class="text-lg font-bold mb-2">30-Day Returns</h4>
                        <p class="text-slate-500 text-sm">Hassle-free returns on all unused parts in original packaging.</p>
                    </div>
                </div>
            </div>
        </div>

        <!-- ================= SHOP VIEW (CATALOG) ================= -->
        <div id="view-shop" class="view-section py-8 bg-gray-50">
            <div class="container mx-auto px-4">
                <!-- Breadcrumbs -->
                <div class="text-sm text-slate-500 mb-6">
                    <a href="#" onclick="navigate('home')" class="hover:text-brand-accent">Home</a> / <span class="text-slate-800 font-medium" id="shop-page-title">All Products</span>
                </div>

                <div class="flex flex-col lg:flex-row gap-8">
                    <!-- Filters Sidebar -->
                    <aside class="w-full lg:w-64 flex-shrink-0">
                        <div class="bg-white p-5 rounded-xl shadow-sm border border-slate-200 lg:sticky lg:top-24">
                            <div class="flex justify-between items-center mb-4 lg:hidden">
                                <h3 class="font-bold text-lg">Filters</h3>
                                <button onclick="document.getElementById('filter-content').classList.toggle('hidden')" class="text-slate-500">
                                    <i class="fas fa-sliders-h"></i>
                                </button>
                            </div>
                            
                            <div id="filter-content" class="hidden lg:block space-y-6">
                                <!-- Categories -->
                                <div>
                                    <h4 class="font-bold text-slate-900 mb-3 border-b pb-2">Categories</h4>
                                    <ul class="space-y-2 text-sm text-slate-600" id="filter-categories">
                                        <!-- Populated by JS -->
                                    </ul>
                                </div>
                                
                                <!-- Price Range -->
                                <div>
                                    <h4 class="font-bold text-slate-900 mb-3 border-b pb-2">Price Range</h4>
                                    <div class="mt-4">
                                        <input type="range" min="0" max="500" value="500" class="range-slider mb-4" id="price-slider" oninput="updatePriceLabel(this.value)">
                                        <div class="flex justify-between text-sm">
                                            <span>$0</span>
                                            <span class="font-bold text-brand-accent" id="price-label">$500+</span>
                                        </div>
                                    </div>
                                    <button onclick="applyFilters()" class="w-full mt-4 bg-slate-900 text-white py-2 rounded-lg text-sm hover:bg-slate-800 transition">Apply Filters</button>
                                </div>
                            </div>
                        </div>
                    </aside>

                    <!-- Product Grid Area -->
                    <div class="flex-1">
                        <div class="flex justify-between items-center mb-6 bg-white p-4 rounded-xl shadow-sm border border-slate-200">
                            <p class="text-slate-500 text-sm" id="shop-result-count">Showing results</p>
                            <select id="sort-select" onchange="applyFilters()" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-brand-accent focus:border-brand-accent block p-2">
                                <option value="default">Sort by: Default</option>
                                <option value="price-low">Price: Low to High</option>
                                <option value="price-high">Price: High to Low</option>
                                <option value="rating">Top Rated</option>
                            </select>
                        </div>
                        
                        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6" id="shop-product-grid">
                            <!-- Populated by JS -->
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- ================= PRODUCT DETAILS VIEW ================= -->
        <div id="view-product-details" class="view-section py-12 bg-white">
            <div class="container mx-auto px-4">
                 <button onclick="navigate('shop')" class="text-sm text-slate-500 hover:text-brand-accent mb-6 flex items-center"><i class="fas fa-arrow-left mr-2"></i>Back to Shop</button>
                 
                 <div id="product-detail-content" class="grid grid-cols-1 md:grid-cols-2 gap-12">
                     <!-- Populated by JS -->
                 </div>
            </div>
        </div>

        <!-- ================= CART VIEW ================= -->
        <div id="view-cart" class="view-section py-12 bg-gray-50">
            <div class="container mx-auto px-4 max-w-5xl">
                <h2 class="text-3xl font-bold mb-8">Shopping Cart</h2>
                
                <div class="flex flex-col lg:flex-row gap-8">
                    <!-- Cart Items -->
                    <div class="flex-1 bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden">
                        <div id="cart-items-container" class="divide-y divide-slate-100">
                            <!-- Populated by JS -->
                        </div>
                    </div>
                    
                    <!-- Order Summary -->
                    <div class="w-full lg:w-80 flex-shrink-0">
                        <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200 sticky top-24">
                            <h3 class="font-bold text-lg mb-4 border-b pb-4">Order Summary</h3>
                            <div class="space-y-3 text-sm mb-4 border-b pb-4">
                                <div class="flex justify-between"><span class="text-slate-500">Subtotal</span><span class="font-medium" id="cart-subtotal">$0.00</span></div>
                                <div class="flex justify-between"><span class="text-slate-500">Shipping</span><span class="font-medium text-green-600" id="cart-shipping">Free</span></div>
                                <div class="flex justify-between"><span class="text-slate-500">Tax (8%)</span><span class="font-medium" id="cart-tax">$0.00</span></div>
                            </div>
                            <div class="flex justify-between font-bold text-lg mb-6">
                                <span>Total</span><span id="cart-total-final">$0.00</span>
                            </div>
                            <button onclick="navigate('checkout')" id="btn-checkout" class="w-full bg-brand-accent hover:bg-brand-accentHover text-white py-3 rounded-lg font-bold transition flex justify-center items-center gap-2">
                                Proceed to Checkout <i class="fas fa-arrow-right"></i>
                            </button>
                            <p class="text-xs text-center text-slate-400 mt-4"><i class="fas fa-lock mr-1"></i>Secure Encrypted Checkout</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- ================= CHECKOUT VIEW ================= -->
        <div id="view-checkout" class="view-section py-12 bg-gray-50">
            <div class="container mx-auto px-4 max-w-4xl">
                <h2 class="text-3xl font-bold mb-8">Checkout</h2>
                
                <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                    <div class="md:col-span-2 space-y-6">
                        <!-- Shipping Form -->
                        <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                            <h3 class="font-bold text-lg mb-4 flex items-center gap-2"><i class="fas fa-map-marker-alt text-brand-accent"></i> Shipping Address</h3>
                            <form class="grid grid-cols-2 gap-4" onsubmit="event.preventDefault()">
                                <div class="col-span-2 sm:col-span-1">
                                    <label class="block text-sm text-slate-600 mb-1">First Name</label>
                                    <input type="text" class="w-full border border-slate-300 rounded-lg p-2 focus:border-brand-accent outline-none" required>
                                </div>
                                <div class="col-span-2 sm:col-span-1">
                                    <label class="block text-sm text-slate-600 mb-1">Last Name</label>
                                    <input type="text" class="w-full border border-slate-300 rounded-lg p-2 focus:border-brand-accent outline-none" required>
                                </div>
                                <div class="col-span-2">
                                    <label class="block text-sm text-slate-600 mb-1">Address</label>
                                    <input type="text" class="w-full border border-slate-300 rounded-lg p-2 focus:border-brand-accent outline-none" required>
                                </div>
                                <div class="col-span-2 sm:col-span-1">
                                    <label class="block text-sm text-slate-600 mb-1">City</label>
                                    <input type="text" class="w-full border border-slate-300 rounded-lg p-2 focus:border-brand-accent outline-none" required>
                                </div>
                                <div class="col-span-1">
                                    <label class="block text-sm text-slate-600 mb-1">ZIP Code</label>
                                    <input type="text" class="w-full border border-slate-300 rounded-lg p-2 focus:border-brand-accent outline-none" required>
                                </div>
                            </form>
                        </div>
                        
                        <!-- Payment Mock -->
                        <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                            <h3 class="font-bold text-lg mb-4 flex items-center gap-2"><i class="fas fa-credit-card text-brand-accent"></i> Payment Method</h3>
                            <div class="border border-slate-200 rounded-lg p-4 bg-slate-50 mb-4">
                                <label class="flex items-center gap-3 cursor-pointer">
                                    <input type="radio" name="payment" checked class="w-4 h-4 text-brand-accent focus:ring-brand-accent">
                                    <span class="font-medium">Credit/Debit Card</span>
                                    <div class="ml-auto flex gap-1 text-xl">
                                        <i class="fab fa-cc-visa text-blue-800"></i>
                                        <i class="fab fa-cc-mastercard text-orange-600"></i>
                                    </div>
                                </label>
                                <div class="mt-4 grid grid-cols-2 gap-4">
                                    <div class="col-span-2">
                                        <input type="text" placeholder="Card Number" class="w-full border border-slate-300 rounded-lg p-2 focus:border-brand-accent outline-none">
                                    </div>
                                    <input type="text" placeholder="MM/YY" class="border border-slate-300 rounded-lg p-2 focus:border-brand-accent outline-none">
                                    <input type="text" placeholder="CVC" class="border border-slate-300 rounded-lg p-2 focus:border-brand-accent outline-none">
                                </div>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Checkout Summary -->
                    <div class="md:col-span-1">
                         <div class="bg-white p-6 rounded-xl shadow-sm border border-slate-200 sticky top-24">
                            <h3 class="font-bold text-lg mb-4">Payable Amount</h3>
                            <div class="text-3xl font-bold text-brand-accent mb-6" id="checkout-final-total">$0.00</div>
                            <button onclick="placeOrder()" class="w-full bg-slate-900 hover:bg-slate-800 text-white py-3 rounded-lg font-bold transition mb-3">
                                Place Order
                            </button>
                            <p class="text-xs text-slate-500 text-center">By placing your order, you agree to our Terms & Conditions.</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- ================= AUTH VIEW (LOGIN/REGISTER) ================= -->
        <div id="view-auth" class="view-section py-16 bg-gray-50 flex items-center justify-center">
            <div class="bg-white w-full max-w-md p-8 rounded-2xl shadow-lg border border-slate-100">
                <div class="text-center mb-8">
                    <div class="bg-brand-accent text-white w-12 h-12 rounded-full flex items-center justify-center mx-auto mb-4">
                        <i class="fas fa-user text-xl"></i>
                    </div>
                    <h2 class="text-2xl font-bold" id="auth-title">Welcome Back</h2>
                    <p class="text-slate-500 text-sm mt-1">Sign in to your AutoParts Hub account</p>
                </div>
                
                <form onsubmit="handleAuth(event)" class="space-y-4">
                    <div>
                        <label class="block text-sm font-medium text-slate-700 mb-1">Email Address</label>
                        <input type="email" id="auth-email" class="w-full border border-slate-300 rounded-lg p-3 focus:ring-2 focus:ring-brand-accent/20 focus:border-brand-accent outline-none transition" placeholder="name@example.com" required value="customer@test.com">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-slate-700 mb-1">Password</label>
                        <input type="password" id="auth-pass" class="w-full border border-slate-300 rounded-lg p-3 focus:ring-2 focus:ring-brand-accent/20 focus:border-brand-accent outline-none transition" placeholder="••••••••" required value="password">
                    </div>
                    
                    <div class="flex items-center justify-between text-sm">
                        <label class="flex items-center text-slate-600">
                            <input type="checkbox" class="mr-2 rounded text-brand-accent focus:ring-brand-accent"> Remember me
                        </label>
                        <a href="#" class="text-brand-accent hover:underline">Forgot password?</a>
                    </div>
                    
                    <button type="submit" class="w-full bg-brand-dark hover:bg-slate-800 text-white font-bold py-3 rounded-lg transition mt-4">
                        Sign In
                    </button>
                </form>
                
                <div class="mt-6 text-center text-sm text-slate-500">
                    <p>Don't have an account? <a href="#" class="text-brand-accent font-semibold hover:underline">Register here</a></p>
                    <div class="mt-4 pt-4 border-t border-slate-100 text-xs text-slate-400">
                        <p>Demo Admin Login: admin@autoparts.com / admin</p>
                    </div>
                </div>
            </div>
        </div>

        <!-- ================= USER DASHBOARD VIEW ================= -->
        <div id="view-dashboard" class="view-section py-12 bg-gray-50">
            <div class="container mx-auto px-4 max-w-6xl">
                <div class="flex justify-between items-end mb-8 border-b pb-4">
                    <div>
                        <h2 class="text-3xl font-bold">My Account</h2>
                        <p class="text-slate-500" id="dash-greeting">Welcome, User</p>
                    </div>
                    <button onclick="logout()" class="text-red-500 hover:text-red-700 font-medium text-sm flex items-center gap-1">
                        <i class="fas fa-sign-out-alt"></i> Logout
                    </button>
                </div>
                
                <div class="flex flex-col md:flex-row gap-8">
                    <!-- Sidebar Nav -->
                    <div class="w-full md:w-64 flex-shrink-0">
                        <div class="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden">
                            <button onclick="switchDashTab('orders')" id="tab-orders" class="dash-tab w-full text-left px-5 py-4 border-b border-slate-100 hover:bg-slate-50 font-medium text-brand-accent bg-slate-50 border-l-4 border-l-brand-accent">
                                <i class="fas fa-box w-6"></i> Order History
                            </button>
                            <button onclick="switchDashTab('wishlist')" id="tab-wishlist" class="dash-tab w-full text-left px-5 py-4 border-b border-slate-100 hover:bg-slate-50 font-medium text-slate-600">
                                <i class="fas fa-heart w-6"></i> My Wishlist
                            </button>
                            <button class="dash-tab w-full text-left px-5 py-4 hover:bg-slate-50 font-medium text-slate-600">
                                <i class="fas fa-cog w-6"></i> Account Settings
                            </button>
                        </div>
                        
                        <!-- Admin Link (Conditionally shown via JS) -->
                        <div id="admin-link-container" class="mt-4 hidden">
                             <button onclick="navigate('admin')" class="w-full bg-slate-900 text-white py-3 rounded-lg flex items-center justify-center gap-2 hover:bg-slate-800 transition shadow-md">
                                <i class="fas fa-shield-alt text-brand-accent"></i> Admin Dashboard
                            </button>
                        </div>
                    </div>
                    
                    <!-- Content Area -->
                    <div class="flex-1">
                        <!-- Orders Tab -->
                        <div id="dash-content-orders" class="bg-white rounded-xl shadow-sm border border-slate-200 p-6">
                            <h3 class="text-xl font-bold mb-4">Order History & Tracking</h3>
                            <div class="overflow-x-auto">
                                <table class="w-full text-left text-sm">
                                    <thead class="bg-slate-50 text-slate-600 uppercase text-xs">
                                        <tr>
                                            <th class="px-4 py-3 rounded-tl-lg">Order ID</th>
                                            <th class="px-4 py-3">Date</th>
                                            <th class="px-4 py-3">Total</th>
                                            <th class="px-4 py-3">Status</th>
                                            <th class="px-4 py-3 rounded-tr-lg">Action</th>
                                        </tr>
                                    </thead>
                                    <tbody id="user-orders-table" class="divide-y divide-slate-100">
                                        <!-- Mock Orders populated by JS -->
                                    </tbody>
                                </table>
                            </div>
                        </div>
                        
                        <!-- Wishlist Tab -->
                        <div id="dash-content-wishlist" class="hidden bg-white rounded-xl shadow-sm border border-slate-200 p-6">
                            <h3 class="text-xl font-bold mb-4">My Wishlist</h3>
                            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4" id="user-wishlist-grid">
                                <!-- Populated by JS -->
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- ================= ADMIN DASHBOARD ================= -->
        <div id="view-admin" class="view-section min-h-screen bg-slate-900 text-slate-300">
            <div class="flex h-screen overflow-hidden">
                <!-- Admin Sidebar -->
                <aside class="w-64 bg-slate-950 border-r border-slate-800 flex flex-col hidden md:flex">
                    <div class="p-4 border-b border-slate-800 flex items-center gap-2">
                        <i class="fas fa-shield-alt text-brand-accent text-xl"></i>
                        <span class="text-white font-bold text-lg">Admin Panel</span>
                    </div>
                    <nav class="flex-1 py-4">
                        <a href="#" class="block px-6 py-3 bg-brand-accent/10 text-brand-accent border-r-2 border-brand-accent"><i class="fas fa-chart-line w-6"></i> Overview</a>
                        <a href="#" class="block px-6 py-3 hover:bg-slate-800 hover:text-white transition"><i class="fas fa-box-open w-6"></i> Products</a>
                        <a href="#" class="block px-6 py-3 hover:bg-slate-800 hover:text-white transition"><i class="fas fa-shopping-cart w-6"></i> Orders</a>
                        <a href="#" class="block px-6 py-3 hover:bg-slate-800 hover:text-white transition"><i class="fas fa-users w-6"></i> Customers</a>
                    </nav>
                    <div class="p-4 border-t border-slate-800">
                         <button onclick="navigate('home')" class="w-full bg-slate-800 hover:bg-slate-700 text-white py-2 rounded flex items-center justify-center gap-2 transition">
                            <i class="fas fa-store"></i> Back to Store
                        </button>
                    </div>
                </aside>
                
                <!-- Admin Content -->
                <main class="flex-1 overflow-y-auto bg-slate-900 p-4 md:p-8">
                    <div class="flex justify-between items-center mb-8">
                        <h2 class="text-2xl font-bold text-white">Dashboard Overview</h2>
                        <div class="flex items-center gap-3">
                            <span class="bg-green-500/20 text-green-400 px-3 py-1 rounded-full text-xs font-bold border border-green-500/30">System Online</span>
                        </div>
                    </div>
                    
                    <!-- Stats Cards -->
                    <div class="grid grid-cols-1 md:grid-cols-4 gap-6 mb-8">
                        <div class="bg-slate-800 p-6 rounded-xl border border-slate-700">
                            <div class="text-slate-400 text-sm font-medium mb-1">Total Revenue</div>
                            <div class="text-3xl font-bold text-white">$24,590</div>
                            <div class="text-green-400 text-xs mt-2"><i class="fas fa-arrow-up"></i> 12% from last month</div>
                        </div>
                        <div class="bg-slate-800 p-6 rounded-xl border border-slate-700">
                            <div class="text-slate-400 text-sm font-medium mb-1">Total Orders</div>
                            <div class="text-3xl font-bold text-white">142</div>
                            <div class="text-green-400 text-xs mt-2"><i class="fas fa-arrow-up"></i> 5% from last month</div>
                        </div>
                        <div class="bg-slate-800 p-6 rounded-xl border border-slate-700">
                            <div class="text-slate-400 text-sm font-medium mb-1">Total Products</div>
                            <div class="text-3xl font-bold text-white" id="admin-total-products">0</div>
                            <div class="text-slate-500 text-xs mt-2">In inventory</div>
                        </div>
                        <div class="bg-slate-800 p-6 rounded-xl border border-slate-700">
                            <div class="text-slate-400 text-sm font-medium mb-1">Low Stock Alerts</div>
                            <div class="text-3xl font-bold text-brand-accent">3</div>
                            <div class="text-brand-accent text-xs mt-2">Requires attention</div>
                        </div>
                    </div>
                    
                    <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                        <!-- Recent Orders Table -->
                        <div class="lg:col-span-2 bg-slate-800 rounded-xl border border-slate-700 overflow-hidden">
                            <div class="p-5 border-b border-slate-700 flex justify-between items-center">
                                <h3 class="font-bold text-white">Recent Orders</h3>
                                <button class="text-xs text-brand-accent hover:text-white transition">View All</button>
                            </div>
                            <div class="overflow-x-auto">
                                <table class="w-full text-left text-sm">
                                    <thead class="bg-slate-950/50 text-slate-400 text-xs">
                                        <tr>
                                            <th class="px-5 py-3">Order ID</th>
                                            <th class="px-5 py-3">Customer</th>
                                            <th class="px-5 py-3">Date</th>
                                            <th class="px-5 py-3">Status</th>
                                            <th class="px-5 py-3">Total</th>
                                        </tr>
                                    </thead>
                                    <tbody id="admin-orders-table" class="divide-y divide-slate-700/50">
                                        <!-- Populated by JS -->
                                    </tbody>
                                </table>
                            </div>
                        </div>
                        
                        <!-- Inventory Snapshot -->
                        <div class="bg-slate-800 rounded-xl border border-slate-700 overflow-hidden">
                             <div class="p-5 border-b border-slate-700 flex justify-between items-center">
                                <h3 class="font-bold text-white">Inventory Snapshot</h3>
                                <button onclick="navigate('shop')" class="text-xs text-brand-accent hover:text-white transition">Catalog</button>
                            </div>
                            <div class="p-5 space-y-4" id="admin-inventory-list">
                                <!-- Populated by JS -->
                            </div>
                        </div>
                    </div>
                </main>
            </div>
        </div>

        <!-- ================= CONTACT PAGE ================= -->
        <div id="view-contact" class="view-section py-16 bg-white">
            <div class="container mx-auto px-4 max-w-6xl">
                <div class="text-center mb-12">
                    <h2 class="text-4xl font-bold mb-4">Get in Touch</h2>
                    <p class="text-slate-500 max-w-2xl mx-auto">Have questions about a part, fitment, or an existing order? Our automotive experts are here to help you.</p>
                </div>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-12">
                    <!-- Contact Info & Quick Actions -->
                    <div>
                        <div class="bg-slate-50 p-8 rounded-2xl border border-slate-100 mb-8">
                            <h3 class="text-2xl font-bold mb-6">Contact Information</h3>
                            <ul class="space-y-6">
                                <li class="flex items-start gap-4">
                                    <div class="w-10 h-10 rounded-full bg-brand-accent/10 text-brand-accent flex items-center justify-center flex-shrink-0 text-xl">
                                        <i class="fas fa-map-marker-alt"></i>
                                    </div>
                                    <div>
                                        <h4 class="font-bold text-slate-900">Main Headquarters</h4>
                                        <p class="text-slate-600 mt-1">1234 Automotive Blvd, Suite 100<br>Detroit, MI 48201, USA</p>
                                    </div>
                                </li>
                                <li class="flex items-start gap-4">
                                    <div class="w-10 h-10 rounded-full bg-brand-accent/10 text-brand-accent flex items-center justify-center flex-shrink-0 text-xl">
                                        <i class="fas fa-envelope"></i>
                                    </div>
                                    <div>
                                        <h4 class="font-bold text-slate-900">Email Support</h4>
                                        <p class="text-slate-600 mt-1">support@autopartshub.com<br>sales@autopartshub.com</p>
                                    </div>
                                </li>
                            </ul>
                        </div>
                        
                        <div class="grid grid-cols-2 gap-4">
                            <!-- WhatsApp Button -->
                            <a href="https://wa.me/1234567890" target="_blank" class="bg-[#25D366] hover:bg-[#1ebe57] text-white p-4 rounded-xl flex flex-col items-center justify-center gap-2 transition shadow-md hover:shadow-lg">
                                <i class="fab fa-whatsapp text-3xl"></i>
                                <span class="font-bold">WhatsApp Us</span>
                            </a>
                            <!-- Phone Button -->
                            <a href="tel:+18005550199" class="bg-slate-900 hover:bg-slate-800 text-white p-4 rounded-xl flex flex-col items-center justify-center gap-2 transition shadow-md hover:shadow-lg">
                                <i class="fas fa-phone-alt text-3xl"></i>
                                <span class="font-bold">1-800-555-0199</span>
                            </a>
                        </div>
                    </div>
                    
                    <!-- Contact Form -->
                    <div class="bg-white p-8 rounded-2xl shadow-xl border border-slate-100">
                        <h3 class="text-2xl font-bold mb-6">Send a Message</h3>
                        <form onsubmit="event.preventDefault(); showToast('Message sent successfully!'); this.reset();" class="space-y-4">
                            <div class="grid grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-sm text-slate-600 mb-1">First Name</label>
                                    <input type="text" class="w-full border border-slate-300 rounded-lg p-3 focus:border-brand-accent outline-none bg-slate-50 focus:bg-white transition" required>
                                </div>
                                <div>
                                    <label class="block text-sm text-slate-600 mb-1">Last Name</label>
                                    <input type="text" class="w-full border border-slate-300 rounded-lg p-3 focus:border-brand-accent outline-none bg-slate-50 focus:bg-white transition" required>
                                </div>
                            </div>
                            <div>
                                <label class="block text-sm text-slate-600 mb-1">Email Address</label>
                                <input type="email" class="w-full border border-slate-300 rounded-lg p-3 focus:border-brand-accent outline-none bg-slate-50 focus:bg-white transition" required>
                            </div>
                            <div>
                                <label class="block text-sm text-slate-600 mb-1">Subject / Inquiry Type</label>
                                <select class="w-full border border-slate-300 rounded-lg p-3 focus:border-brand-accent outline-none bg-slate-50 focus:bg-white transition">
                                    <option>Part Fitment Question</option>
                                    <option>Order Status</option>
                                    <option>Returns & Refunds</option>
                                    <option>Other</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-sm text-slate-600 mb-1">Message</label>
                                <textarea rows="4" class="w-full border border-slate-300 rounded-lg p-3 focus:border-brand-accent outline-none bg-slate-50 focus:bg-white transition resize-none" required></textarea>
                            </div>
                            <button type="submit" class="w-full bg-brand-accent hover:bg-brand-accentHover text-white font-bold py-3 rounded-lg transition shadow-md">
                                Send Message
                            </button>
                        </form>
                    </div>
                </div>
            </div>
        </div>

    </main>

    <!-- Footer -->
    <footer id="main-footer" class="bg-brand-darker text-slate-300 pt-16 pb-8 border-t border-slate-800">
        <div class="container mx-auto px-4">
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8 mb-12">
                <!-- Brand Info -->
                <div>
                    <div class="flex items-center gap-2 mb-6">
                        <div class="bg-brand-accent text-white p-2 rounded-lg flex items-center justify-center">
                            <i class="fas fa-car-side text-lg"></i>
                        </div>
                        <h2 class="text-xl font-bold text-white leading-none tracking-tight">AutoParts<span class="text-brand-accent">Hub</span></h2>
                    </div>
                    <p class="text-sm text-slate-400 mb-6 line-clamp-3">Your premier destination for high-quality automotive parts and accessories. We provide OEM and aftermarket solutions for all major vehicle brands.</p>
                    <div class="flex gap-4">
                        <a href="#" class="w-8 h-8 rounded-full bg-slate-800 hover:bg-brand-accent flex items-center justify-center text-white transition"><i class="fab fa-facebook-f"></i></a>
                        <a href="#" class="w-8 h-8 rounded-full bg-slate-800 hover:bg-brand-accent flex items-center justify-center text-white transition"><i class="fab fa-twitter"></i></a>
                        <a href="#" class="w-8 h-8 rounded-full bg-slate-800 hover:bg-brand-accent flex items-center justify-center text-white transition"><i class="fab fa-instagram"></i></a>
                    </div>
                </div>
                
                <!-- Quick Links -->
                <div>
                    <h4 class="text-white font-bold mb-4 uppercase text-sm tracking-wider">Shop</h4>
                    <ul class="space-y-2 text-sm">
                        <li><a href="#" onclick="navigate('shop', {cat: 'Engine'})" class="hover:text-brand-accent transition">Engine Components</a></li>
                        <li><a href="#" onclick="navigate('shop', {cat: 'Brakes'})" class="hover:text-brand-accent transition">Brake Systems</a></li>
                        <li><a href="#" onclick="navigate('shop', {cat: 'Suspension'})" class="hover:text-brand-accent transition">Suspension & Steering</a></li>
                        <li><a href="#" onclick="navigate('shop', {cat: 'Tires'})" class="hover:text-brand-accent transition">Tires & Wheels</a></li>
                        <li><a href="#" onclick="navigate('shop', {cat: 'Accessories'})" class="hover:text-brand-accent transition">Interior Accessories</a></li>
                    </ul>
                </div>
                
                <!-- Customer Service -->
                <div>
                    <h4 class="text-white font-bold mb-4 uppercase text-sm tracking-wider">Customer Service</h4>
                    <ul class="space-y-2 text-sm">
                        <li><a href="#" onclick="navigate('contact')" class="hover:text-brand-accent transition">Contact Us</a></li>
                        <li><a href="#" class="hover:text-brand-accent transition">Shipping Policy</a></li>
                        <li><a href="#" class="hover:text-brand-accent transition">Returns & Exchanges</a></li>
                        <li><a href="#" class="hover:text-brand-accent transition">FAQ</a></li>
                        <li><a href="#" class="hover:text-brand-accent transition">Track Order</a></li>
                    </ul>
                </div>
                
                <!-- Newsletter -->
                <div>
                    <h4 class="text-white font-bold mb-4 uppercase text-sm tracking-wider">Stay Updated</h4>
                    <p class="text-sm text-slate-400 mb-4">Subscribe to our newsletter for exclusive deals and automotive tips.</p>
                    <form onsubmit="event.preventDefault(); showToast('Subscribed successfully!');" class="flex">
                        <input type="email" placeholder="Enter your email" class="w-full bg-slate-800 border-none rounded-l-lg px-4 py-2 text-sm text-white focus:outline-none focus:ring-1 focus:ring-brand-accent" required>
                        <button type="submit" class="bg-brand-accent hover:bg-brand-accentHover text-white px-4 py-2 rounded-r-lg text-sm font-bold transition">
                            Subscribe
                        </button>
                    </form>
                    <div class="mt-6 flex gap-2 text-2xl text-slate-600">
                        <i class="fab fa-cc-visa hover:text-white transition cursor-pointer"></i>
                        <i class="fab fa-cc-mastercard hover:text-white transition cursor-pointer"></i>
                        <i class="fab fa-cc-paypal hover:text-white transition cursor-pointer"></i>
                    </div>
                </div>
            </div>
            
            <div class="pt-8 border-t border-slate-800 text-center text-sm text-slate-500 flex flex-col md:flex-row justify-between items-center">
                <p>&copy; 2026 AutoParts Hub. All rights reserved.</p>
                <div class="flex gap-4 mt-4 md:mt-0">
                    <a href="#" class="hover:text-white transition">Privacy Policy</a>
                    <a href="#" class="hover:text-white transition">Terms of Service</a>
                </div>
            </div>
        </div>
    </footer>

    <script>
        // --- Mock Data ---
        const categories = [
            { name: "Engine", icon: "fa-cogs" },
            { name: "Brakes", icon: "fa-compact-disc" },
            { name: "Suspension", icon: "fa-compress-arrows-alt" },
            { name: "Electrical", icon: "fa-bolt" },
            { name: "Tires", icon: "fa-tire" }, // Using generic icons where specific auto ones lack
            { name: "Body Parts", icon: "fa-car" },
            { name: "Accessories", icon: "fa-gem" }
        ];

        const mockProducts = [
            { id: 1, name: "Premium Ceramic Brake Pads Set", price: 45.99, category: "Brakes", rating: 4.8, reviews: 124, stock: 15, img: "https://placehold.co/400x300/e2e8f0/475569?text=Brake+Pads", desc: "Low dust, ultra-quiet ceramic brake pads for superior stopping power.", featured: true },
            { id: 2, name: "High Performance Oil Filter", price: 12.50, category: "Engine", rating: 4.9, reviews: 342, stock: 50, img: "https://placehold.co/400x300/e2e8f0/475569?text=Oil+Filter", desc: "Advanced synthetic blend media traps 99% of contaminants.", featured: true },
            { id: 3, name: "LED Headlight Bulbs (Pair)", price: 34.99, category: "Electrical", rating: 4.5, reviews: 89, stock: 22, img: "https://placehold.co/400x300/e2e8f0/475569?text=LED+Bulbs", desc: "6000K cool white light, 300% brighter than halogen.", featured: true },
            { id: 4, name: "All-Season Radial Tire 205/55R16", price: 89.00, category: "Tires", rating: 4.7, reviews: 56, stock: 8, img: "https://placehold.co/400x300/e2e8f0/475569?text=Radial+Tire", desc: "Excellent traction in wet and dry conditions. 60k mile warranty.", featured: true },
            { id: 5, name: "Gas-Charged Shock Absorber", price: 55.00, category: "Suspension", rating: 4.6, reviews: 45, stock: 12, img: "https://placehold.co/400x300/e2e8f0/475569?text=Shock+Absorber", desc: "Restores original handling and control.", featured: false },
            { id: 6, name: "12V Automotive Car Battery", price: 129.99, category: "Electrical", rating: 4.8, reviews: 210, stock: 5, img: "https://placehold.co/400x300/e2e8f0/475569?text=Car+Battery", desc: "Reliable starting power in extreme temperatures.", featured: false },
            { id: 7, name: "Synthetic Motor Oil 5W-30 (5 Qt)", price: 28.99, category: "Engine", rating: 4.9, reviews: 500, stock: 100, img: "https://placehold.co/400x300/e2e8f0/475569?text=Motor+Oil", desc: "Maximum engine protection and performance.", featured: false },
            { id: 8, name: "Heavy Duty Floor Mats (4pc)", price: 39.99, category: "Accessories", rating: 4.4, reviews: 78, stock: 30, img: "https://placehold.co/400x300/e2e8f0/475569?text=Floor+Mats", desc: "Deep dish design traps dirt, water, and snow.", featured: false },
            { id: 9, name: "Replacement Side Mirror", price: 42.00, category: "Body Parts", rating: 4.2, reviews: 12, stock: 3, img: "https://placehold.co/400x300/e2e8f0/475569?text=Side+Mirror", desc: "Direct fit replacement for damaged factory mirrors.", featured: false },
            { id: 10, name: "Performance Spark Plugs (Set of 4)", price: 24.99, category: "Electrical", rating: 4.7, reviews: 156, stock: 40, img: "https://placehold.co/400x300/e2e8f0/475569?text=Spark+Plugs", desc: "Iridium core for improved throttle response and fuel efficiency.", featured: false }
        ];

        const mockOrders = [
            { id: "ORD-9482", date: "Oct 01, 2026", total: 145.99, status: "Delivered" },
            { id: "ORD-9321", date: "Sep 15, 2026", total: 45.99, status: "Shipped" },
            { id: "ORD-9102", date: "Aug 02, 2026", total: 329.50, status: "Processing" }
        ];

        // --- Application State ---
        let state = {
            currentView: 'home',
            cart: [], // { product, quantity }
            wishlist: [], // array of product IDs
            user: null, // null or { name, email, role: 'customer'|'admin' }
            currentCategory: 'all',
            searchQuery: ''
        };

        // --- Core Functions ---

        function init() {
            renderHomeCategories();
            renderFeaturedProducts();
            renderFilterCategories();
            updateHeaderBadges();
            
            // Check auth state (mock)
            const savedUser = sessionStorage.getItem('autoUser');
            if(savedUser) {
                state.user = JSON.parse(savedUser);
                updateAuthUI();
            }

            // Handle browser back button
            window.onpopstate = (e) => {
                if(e.state && e.state.view) {
                    showView(e.state.view, false);
                }
            };
            
            // Initial routing
            const urlParams = new URLSearchParams(window.location.search);
            const initialView = urlParams.get('view') || 'home';
            navigate(initialView, {}, false);
        }

        function navigate(viewId, params = {}, pushState = true) {
            // Access control logic
            if(viewId === 'dashboard' && !state.user) {
                showToast("Please login first", "error");
                navigate('auth');
                return;
            }
            if(viewId === 'admin' && (!state.user || state.user.role !== 'admin')) {
                showToast("Access Denied", "error");
                navigate('home');
                return;
            }
            if(viewId === 'checkout' && state.cart.length === 0) {
                showToast("Your cart is empty", "error");
                return;
            }

            // Pre-render setup based on route
            if(viewId === 'shop') {
                state.currentCategory = params.cat || 'all';
                state.searchQuery = params.q || '';
                document.getElementById('global-search').value = state.searchQuery;
                document.getElementById('shop-page-title').innerText = state.currentCategory === 'all' ? 'All Products' : state.currentCategory;
                applyFilters(); // Renders the grid
            } else if (viewId === 'product-details') {
                renderProductDetails(params.id);
            } else if (viewId === 'cart') {
                renderCart();
            } else if (viewId === 'checkout') {
                renderCheckoutSummary();
            } else if (viewId === 'dashboard') {
                renderDashboard();
            } else if (viewId === 'wishlist') {
                if(state.user) {
                    viewId = 'dashboard';
                    renderDashboard();
                    setTimeout(() => switchDashTab('wishlist'), 100);
                } else {
                     showToast("Login to view wishlist");
                     navigate('auth');
                     return;
                }
            } else if (viewId === 'admin') {
                renderAdminDashboard();
            }

            showView(viewId, pushState);
            window.scrollTo(0, 0);
        }

        function showView(viewId, pushState = true) {
            // Hide all views
            document.querySelectorAll('.view-section').forEach(el => {
                el.classList.remove('active');
                // Small delay to allow fade out before display none (handled via CSS class toggle usually, but keeping it simple here)
                setTimeout(() => { if(!el.classList.contains('active')) el.style.display = 'none'; }, 300);
            });

            // Show target view
            const target = document.getElementById(`view-${viewId}`);
            if(target) {
                target.style.display = 'block';
                // Trigger reflow for transition
                void target.offsetWidth; 
                target.classList.add('active');
                state.currentView = viewId;

                // Handle header/footer visibility for Admin
                if(viewId === 'admin') {
                    document.getElementById('main-header').classList.add('hidden');
                    document.getElementById('main-footer').classList.add('hidden');
                } else {
                    document.getElementById('main-header').classList.remove('hidden');
                    document.getElementById('main-footer').classList.remove('hidden');
                }

                if(pushState) {
                    const newUrl = window.location.protocol + "//" + window.location.host + window.location.pathname + `?view=${viewId}`;
                    window.history.pushState({view: viewId}, '', newUrl);
                }
            }
        }

        // --- Render Functions ---

        function generateProductCard(product) {
            const inWishlist = state.wishlist.includes(product.id);
            const heartClass = inWishlist ? 'fas fa-heart text-brand-accent' : 'far fa-heart text-slate-400';
            
            return `
                <div class="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden group hover:shadow-md transition flex flex-col">
                    <div class="relative h-48 bg-slate-100 p-4 flex items-center justify-center overflow-hidden cursor-pointer" onclick="navigate('product-details', {id: ${product.id}})">
                        <img src="${product.img}" alt="${product.name}" class="max-h-full object-contain mix-blend-multiply group-hover:scale-105 transition-transform duration-300">
                        <button onclick="event.stopPropagation(); toggleWishlist(${product.id})" class="absolute top-3 right-3 w-8 h-8 bg-white rounded-full flex items-center justify-center shadow-sm hover:bg-slate-50 transition z-10">
                            <i class="${heartClass}"></i>
                        </button>
                        ${product.stock < 10 ? `<span class="absolute top-3 left-3 bg-red-100 text-red-600 text-[10px] font-bold px-2 py-1 rounded">Only ${product.stock} left</span>` : ''}
                    </div>
                    <div class="p-4 flex-1 flex flex-col">
                        <div class="text-xs text-slate-400 mb-1 font-medium uppercase tracking-wider">${product.category}</div>
                        <h4 class="font-bold text-slate-800 text-sm mb-2 line-clamp-2 hover:text-brand-accent cursor-pointer flex-1" onclick="navigate('product-details', {id: ${product.id}})">${product.name}</h4>
                        <div class="flex items-center gap-1 text-yellow-400 text-xs mb-3">
                            <i class="fas fa-star"></i><span class="text-slate-600 font-medium ml-1">${product.rating}</span> <span class="text-slate-400">(${product.reviews})</span>
                        </div>
                        <div class="flex justify-between items-center mt-auto pt-3 border-t border-slate-100">
                            <div class="text-lg font-bold text-slate-900">$${product.price.toFixed(2)}</div>
                            <button onclick="addToCart(${product.id})" class="bg-slate-900 hover:bg-brand-accent text-white w-9 h-9 rounded-lg flex items-center justify-center transition" title="Add to Cart">
                                <i class="fas fa-plus"></i>
                            </button>
                        </div>
                    </div>
                </div>
            `;
        }

        function renderHomeCategories() {
            const container = document.getElementById('home-categories-grid');
            container.innerHTML = categories.map(cat => `
                <div onclick="navigate('shop', {cat: '${cat.name}'})" class="bg-white border border-slate-200 rounded-xl p-4 text-center cursor-pointer hover:border-brand-accent hover:shadow-md transition group">
                    <div class="w-12 h-12 mx-auto bg-slate-50 text-slate-600 rounded-full flex items-center justify-center text-xl mb-3 group-hover:bg-brand-accent group-hover:text-white transition">
                        <i class="fas ${cat.icon}"></i>
                    </div>
                    <h4 class="font-medium text-sm text-slate-800">${cat.name}</h4>
                </div>
            `).join('');
        }

        function renderFeaturedProducts() {
            const container = document.getElementById('home-featured-products');
            const featured = mockProducts.filter(p => p.featured).slice(0, 4);
            container.innerHTML = featured.map(generateProductCard).join('');
        }

        function renderFilterCategories() {
            const container = document.getElementById('filter-categories');
            container.innerHTML = `
                <li>
                    <label class="flex items-center gap-2 cursor-pointer group">
                        <input type="radio" name="cat_filter" value="all" ${state.currentCategory === 'all' ? 'checked' : ''} onchange="updateCategoryFilter('all')" class="text-brand-accent focus:ring-brand-accent">
                        <span class="group-hover:text-brand-accent transition">All Parts</span>
                    </label>
                </li>
            ` + categories.map(cat => `
                <li>
                    <label class="flex items-center gap-2 cursor-pointer group">
                        <input type="radio" name="cat_filter" value="${cat.name}" ${state.currentCategory === cat.name ? 'checked' : ''} onchange="updateCategoryFilter('${cat.name}')" class="text-brand-accent focus:ring-brand-accent">
                        <span class="group-hover:text-brand-accent transition">${cat.name}</span>
                    </label>
                </li>
            `).join('');
        }

        function updateCategoryFilter(cat) {
            state.currentCategory = cat;
            document.getElementById('shop-page-title').innerText = cat === 'all' ? 'All Products' : cat;
            applyFilters();
        }

        function updatePriceLabel(val) {
            document.getElementById('price-label').innerText = `$${val}${val == 500 ? '+' : ''}`;
        }

        function performSearch(source = 'desktop') {
            const inputId = source === 'desktop' ? 'global-search' : 'global-search-mobile';
            const query = document.getElementById(inputId).value;
            if(source === 'mobile') document.getElementById('mobile-search').classList.add('hidden');
            navigate('shop', {q: query, cat: 'all'});
        }

        function applyFilters() {
            const maxPrice = document.getElementById('price-slider').value;
            const sortVal = document.getElementById('sort-select').value;
            
            let filtered = mockProducts;

            // Category Filter
            if(state.currentCategory !== 'all') {
                filtered = filtered.filter(p => p.category === state.currentCategory);
            }

            // Search Filter
            if(state.searchQuery) {
                const q = state.searchQuery.toLowerCase();
                filtered = filtered.filter(p => p.name.toLowerCase().includes(q) || p.category.toLowerCase().includes(q));
            }

            // Price Filter
            if(maxPrice < 500) {
                filtered = filtered.filter(p => p.price <= maxPrice);
            }

            // Sorting
            if(sortVal === 'price-low') filtered.sort((a,b) => a.price - b.price);
            else if(sortVal === 'price-high') filtered.sort((a,b) => b.price - a.price);
            else if(sortVal === 'rating') filtered.sort((a,b) => b.rating - a.rating);

            // Render
            const container = document.getElementById('shop-product-grid');
            document.getElementById('shop-result-count').innerText = `Showing ${filtered.length} results`;
            
            if(filtered.length === 0) {
                container.innerHTML = `<div class="col-span-full text-center py-12 text-slate-500"><i class="fas fa-search text-3xl mb-3"></i><p>No products found matching your criteria.</p><button onclick="navigate('shop', {cat:'all'})" class="mt-4 text-brand-accent underline">Clear Filters</button></div>`;
            } else {
                container.innerHTML = filtered.map(generateProductCard).join('');
            }
        }

        function renderProductDetails(id) {
            const product = mockProducts.find(p => p.id === parseInt(id));
            if(!product) return navigate('shop');

            const inWishlist = state.wishlist.includes(product.id);
            const container = document.getElementById('product-detail-content');
            
            container.innerHTML = `
                <!-- Image Gallery -->
                <div class="bg-slate-50 rounded-2xl p-8 flex items-center justify-center border border-slate-100 relative">
                     <button onclick="toggleWishlist(${product.id})" class="absolute top-4 right-4 w-10 h-10 bg-white rounded-full flex items-center justify-center shadow hover:bg-slate-50 transition z-10 text-xl">
                        <i class="${inWishlist ? 'fas fa-heart text-brand-accent' : 'far fa-heart text-slate-400'}"></i>
                    </button>
                    <img src="${product.img}" alt="${product.name}" class="max-w-full mix-blend-multiply drop-shadow-xl">
                </div>
                
                <!-- Details -->
                <div class="flex flex-col">
                    <div class="text-sm font-bold text-brand-accent uppercase tracking-wider mb-2">${product.category}</div>
                    <h2 class="text-3xl font-bold text-slate-900 mb-2">${product.name}</h2>
                    
                    <div class="flex items-center gap-4 mb-6">
                        <div class="flex items-center text-yellow-400">
                            <i class="fas fa-star"></i><i class="fas fa-star"></i><i class="fas fa-star"></i><i class="fas fa-star"></i><i class="fas fa-star-half-alt"></i>
                        </div>
                        <span class="text-sm text-slate-500">${product.rating} Rating (${product.reviews} Reviews)</span>
                        <span class="text-sm px-2 py-1 bg-green-100 text-green-700 rounded font-medium"><i class="fas fa-check-circle mr-1"></i> In Stock (${product.stock})</span>
                    </div>
                    
                    <div class="text-4xl font-bold text-slate-900 mb-6">$${product.price.toFixed(2)}</div>
                    
                    <p class="text-slate-600 mb-8 leading-relaxed">${product.desc} Designed to meet OEM specifications for a perfect fit and reliable performance. This part guarantees durability under extreme conditions.</p>
                    
                    <div class="flex items-center gap-4 mb-8">
                        <div class="flex border border-slate-300 rounded-lg overflow-hidden h-12 w-32">
                            <button onclick="updateQtyInput(-1)" class="w-10 bg-slate-50 hover:bg-slate-100 flex items-center justify-center border-r border-slate-300 text-slate-600"><i class="fas fa-minus"></i></button>
                            <input type="number" id="detail-qty" value="1" min="1" max="${product.stock}" class="w-full text-center outline-none font-bold text-slate-800" readonly>
                            <button onclick="updateQtyInput(1)" class="w-10 bg-slate-50 hover:bg-slate-100 flex items-center justify-center border-l border-slate-300 text-slate-600"><i class="fas fa-plus"></i></button>
                        </div>
                        <button onclick="addToCart(${product.id}, parseInt(document.getElementById('detail-qty').value))" class="flex-1 bg-brand-accent hover:bg-brand-accentHover text-white h-12 rounded-lg font-bold transition flex items-center justify-center gap-2 shadow-lg shadow-brand-accent/30">
                            <i class="fas fa-shopping-cart"></i> Add to Cart
                        </button>
                    </div>
                    
                    <div class="border-t border-slate-200 pt-6 mt-auto">
                        <div class="grid grid-cols-2 gap-4 text-sm text-slate-600">
                            <div class="flex items-center gap-2"><i class="fas fa-truck text-slate-400 w-5"></i> Free shipping over $100</div>
                            <div class="flex items-center gap-2"><i class="fas fa-undo text-slate-400 w-5"></i> 30-day return policy</div>
                            <div class="flex items-center gap-2"><i class="fas fa-shield-alt text-slate-400 w-5"></i> 1 Year Warranty</div>
                            <div class="flex items-center gap-2"><i class="fas fa-headset text-slate-400 w-5"></i> 24/7 Expert Support</div>
                        </div>
                    </div>
                </div>
            `;
        }

        function updateQtyInput(change) {
            const input = document.getElementById('detail-qty');
            let val = parseInt(input.value) + change;
            if(val < 1) val = 1;
            input.value = val;
        }

        function addToCart(productId, qty = 1) {
            const product = mockProducts.find(p => p.id === productId);
            const existing = state.cart.find(item => item.product.id === productId);
            
            if(existing) {
                if(existing.quantity + qty > product.stock) {
                    showToast("Not enough stock!", "error");
                    return;
                }
                existing.quantity += qty;
            } else {
                state.cart.push({ product, quantity: qty });
            }
            
            updateHeaderBadges();
            showToast("Added to cart!");
            
            // If on cart page, re-render
            if(state.currentView === 'cart') renderCart();
        }

        function removeFromCart(productId) {
            state.cart = state.cart.filter(item => item.product.id !== productId);
            updateHeaderBadges();
            renderCart();
        }

        function updateCartQty(productId, change) {
            const item = state.cart.find(i => i.product.id === productId);
            if(item) {
                const newQty = item.quantity + change;
                if(newQty > 0 && newQty <= item.product.stock) {
                    item.quantity = newQty;
                    updateHeaderBadges();
                    renderCart();
                    if(state.currentView === 'checkout') renderCheckoutSummary();
                } else if (newQty === 0) {
                    removeFromCart(productId);
                }
            }
        }

        function calculateCartTotal() {
            return state.cart.reduce((sum, item) => sum + (item.product.price * item.quantity), 0);
        }

        function updateHeaderBadges() {
            const cartCount = state.cart.reduce((sum, item) => sum + item.quantity, 0);
            const cartTotal = calculateCartTotal();
            
            const cBadge = document.getElementById('cart-badge');
            if(cartCount > 0) {
                cBadge.innerText = cartCount;
                cBadge.classList.remove('hidden');
                document.getElementById('cart-total-nav').innerText = `$${cartTotal.toFixed(2)}`;
            } else {
                cBadge.classList.add('hidden');
                document.getElementById('cart-total-nav').innerText = `$0.00`;
            }

            const wBadge = document.getElementById('wishlist-badge');
            if(state.wishlist.length > 0) {
                wBadge.innerText = state.wishlist.length;
                wBadge.classList.remove('hidden');
            } else {
                wBadge.classList.add('hidden');
            }
        }

        function renderCart() {
            const container = document.getElementById('cart-items-container');
            const subtotalEl = document.getElementById('cart-subtotal');
            const taxEl = document.getElementById('cart-tax');
            const finalTotalEl = document.getElementById('cart-total-final');
            const checkoutBtn = document.getElementById('btn-checkout');

            if(state.cart.length === 0) {
                container.innerHTML = `
                    <div class="p-12 text-center text-slate-500">
                        <i class="fas fa-shopping-basket text-5xl mb-4 text-slate-300"></i>
                        <h3 class="text-xl font-bold text-slate-700 mb-2">Your cart is empty</h3>
                        <p class="mb-6">Looks like you haven't added any parts yet.</p>
                        <button onclick="navigate('shop')" class="bg-slate-900 text-white px-6 py-2 rounded-lg font-medium hover:bg-slate-800 transition">Start Shopping</button>
                    </div>`;
                subtotalEl.innerText = "$0.00"; taxEl.innerText = "$0.00"; finalTotalEl.innerText = "$0.00";
                checkoutBtn.disabled = true;
                checkoutBtn.classList.add('opacity-50', 'cursor-not-allowed');
                return;
            }

            checkoutBtn.disabled = false;
            checkoutBtn.classList.remove('opacity-50', 'cursor-not-allowed');

            container.innerHTML = state.cart.map(item => `
                <div class="p-4 sm:p-6 flex flex-col sm:flex-row items-center gap-4 hover:bg-slate-50 transition">
                    <img src="${item.product.img}" alt="${item.product.name}" class="w-24 h-24 object-contain bg-slate-100 rounded-lg mix-blend-multiply cursor-pointer" onclick="navigate('product-details', {id: ${item.product.id}})">
                    <div class="flex-1 text-center sm:text-left">
                        <div class="text-xs text-slate-400 font-medium uppercase mb-1">${item.product.category}</div>
                        <h4 class="font-bold text-slate-900 cursor-pointer hover:text-brand-accent" onclick="navigate('product-details', {id: ${item.product.id}})">${item.product.name}</h4>
                        <div class="font-bold text-brand-accent mt-1">$${item.product.price.toFixed(2)}</div>
                    </div>
                    <div class="flex items-center gap-4 mt-4 sm:mt-0">
                        <div class="flex border border-slate-300 rounded overflow-hidden">
                            <button onclick="updateCartQty(${item.product.id}, -1)" class="w-8 h-8 bg-slate-50 flex items-center justify-center hover:bg-slate-200"><i class="fas fa-minus text-xs"></i></button>
                            <input type="text" value="${item.quantity}" readonly class="w-10 h-8 text-center text-sm font-bold border-x border-slate-300 outline-none">
                            <button onclick="updateCartQty(${item.product.id}, 1)" class="w-8 h-8 bg-slate-50 flex items-center justify-center hover:bg-slate-200"><i class="fas fa-plus text-xs"></i></button>
                        </div>
                        <div class="w-20 text-right font-bold text-slate-900 hidden sm:block">$${(item.product.price * item.quantity).toFixed(2)}</div>
                        <button onclick="removeFromCart(${item.product.id})" class="text-red-400 hover:text-red-600 p-2"><i class="fas fa-trash"></i></button>
                    </div>
                </div>
            `).join('');

            const subtotal = calculateCartTotal();
            const tax = subtotal * 0.08;
            const total = subtotal + tax; // Free shipping logic simplified

            subtotalEl.innerText = `$${subtotal.toFixed(2)}`;
            taxEl.innerText = `$${tax.toFixed(2)}`;
            finalTotalEl.innerText = `$${total.toFixed(2)}`;
        }

        function renderCheckoutSummary() {
            const subtotal = calculateCartTotal();
            const tax = subtotal * 0.08;
            const total = subtotal + tax;
            document.getElementById('checkout-final-total').innerText = `$${total.toFixed(2)}`;
        }

        function placeOrder() {
            if(!state.user) {
                showToast("Please login to place an order");
                navigate('auth');
                return;
            }
            // Simulate processing
            showToast("Processing Payment...");
            setTimeout(() => {
                showToast("Order placed successfully!", "success");
                state.cart = [];
                updateHeaderBadges();
                navigate('dashboard');
                switchDashTab('orders');
            }, 1500);
        }

        function toggleWishlist(productId) {
            const idx = state.wishlist.indexOf(productId);
            if(idx > -1) {
                state.wishlist.splice(idx, 1);
                showToast("Removed from wishlist");
            } else {
                state.wishlist.push(productId);
                showToast("Added to wishlist");
            }
            updateHeaderBadges();
            
            // Re-render views if active
            if(state.currentView === 'shop') applyFilters();
            else if(state.currentView === 'home') renderFeaturedProducts();
            else if(state.currentView === 'dashboard') renderWishlistTab();
            else if(state.currentView === 'product-details') renderProductDetails(productId);
        }

        function handleAuth(e) {
            e.preventDefault();
            const email = document.getElementById('auth-email').value;
            const pass = document.getElementById('auth-pass').value;
            
            // Mock Auth Logic
            if(email.includes('admin') && pass === 'admin') {
                state.user = { name: 'Admin User', email, role: 'admin' };
            } else {
                state.user = { name: email.split('@')[0], email, role: 'customer' };
            }
            
            sessionStorage.setItem('autoUser', JSON.stringify(state.user));
            updateAuthUI();
            showToast("Logged in successfully");
            
            if(state.user.role === 'admin') navigate('admin');
            else navigate('dashboard');
        }

        function logout() {
            state.user = null;
            sessionStorage.removeItem('autoUser');
            updateAuthUI();
            showToast("Logged out");
            navigate('home');
        }

        function updateAuthUI() {
            const greeting = document.getElementById('user-greeting');
            const adminLink = document.getElementById('admin-link-container');
            
            if(state.user) {
                greeting.innerText = state.user.name;
                if(state.user.role === 'admin' && adminLink) adminLink.classList.remove('hidden');
            } else {
                greeting.innerText = 'Sign In';
                if(adminLink) adminLink.classList.add('hidden');
            }
        }

        function renderDashboard() {
            document.getElementById('dash-greeting').innerText = `Welcome back, ${state.user.name}`;
            
            // Render Orders
            const tbody = document.getElementById('user-orders-table');
            tbody.innerHTML = mockOrders.map(o => `
                <tr class="hover:bg-slate-50">
                    <td class="px-4 py-3 font-medium text-slate-900">${o.id}</td>
                    <td class="px-4 py-3">${o.date}</td>
                    <td class="px-4 py-3 font-medium">$${o.total.toFixed(2)}</td>
                    <td class="px-4 py-3">
                        <span class="px-2 py-1 text-xs rounded-full font-medium ${o.status==='Delivered' ? 'bg-green-100 text-green-700' : (o.status==='Shipped'?'bg-blue-100 text-blue-700':'bg-amber-100 text-amber-700')}">
                            ${o.status}
                        </span>
                    </td>
                    <td class="px-4 py-3"><button class="text-brand-accent hover:underline text-xs">Track</button></td>
                </tr>
            `).join('');

            renderWishlistTab();
        }

        function renderWishlistTab() {
            const container = document.getElementById('user-wishlist-grid');
            if(state.wishlist.length === 0) {
                container.innerHTML = `<div class="col-span-full py-8 text-center text-slate-500">Your wishlist is empty.</div>`;
                return;
            }
            
            const wItems = mockProducts.filter(p => state.wishlist.includes(p.id));
            container.innerHTML = wItems.map(p => `
                 <div class="flex items-center gap-4 border border-slate-100 p-3 rounded-lg hover:shadow-sm">
                    <img src="${p.img}" class="w-16 h-16 object-contain mix-blend-multiply bg-slate-50 rounded cursor-pointer" onclick="navigate('product-details', {id:${p.id}})">
                    <div class="flex-1">
                        <h5 class="font-bold text-sm line-clamp-1 hover:text-brand-accent cursor-pointer" onclick="navigate('product-details', {id:${p.id}})">${p.name}</h5>
                        <div class="text-brand-accent font-medium text-sm">$${p.price.toFixed(2)}</div>
                    </div>
                    <div class="flex flex-col gap-2">
                         <button onclick="addToCart(${p.id})" class="bg-slate-900 text-white w-8 h-8 rounded flex items-center justify-center hover:bg-slate-800"><i class="fas fa-cart-plus text-xs"></i></button>
                         <button onclick="toggleWishlist(${p.id})" class="bg-red-50 text-red-500 w-8 h-8 rounded flex items-center justify-center hover:bg-red-100"><i class="fas fa-trash text-xs"></i></button>
                    </div>
                </div>
            `).join('');
        }

        function switchDashTab(tabId) {
            // Update buttons
            document.querySelectorAll('.dash-tab').forEach(el => {
                el.classList.remove('text-brand-accent', 'bg-slate-50', 'border-l-4', 'border-l-brand-accent');
                el.classList.add('text-slate-600');
            });
            const activeBtn = document.getElementById(`tab-${tabId}`);
            if(activeBtn) {
                 activeBtn.classList.remove('text-slate-600');
                 activeBtn.classList.add('text-brand-accent', 'bg-slate-50', 'border-l-4', 'border-l-brand-accent');
            }
            
            // Show content
            document.getElementById('dash-content-orders').classList.add('hidden');
            document.getElementById('dash-content-wishlist').classList.add('hidden');
            
            document.getElementById(`dash-content-${tabId}`).classList.remove('hidden');
        }

        function renderAdminDashboard() {
            document.getElementById('admin-total-products').innerText = mockProducts.length;
            
            // Render Recent Orders
            const tbody = document.getElementById('admin-orders-table');
            tbody.innerHTML = mockOrders.map(o => `
                <tr class="hover:bg-slate-700/30 transition">
                    <td class="px-5 py-3 text-white font-medium">#${o.id}</td>
                    <td class="px-5 py-3">Customer Name</td>
                    <td class="px-5 py-3">${o.date}</td>
                    <td class="px-5 py-3">
                        <span class="px-2 py-1 text-[10px] uppercase tracking-wider rounded border ${o.status==='Delivered' ? 'border-green-500/50 text-green-400 bg-green-500/10' : 'border-amber-500/50 text-amber-400 bg-amber-500/10'}">
                            ${o.status}
                        </span>
                    </td>
                    <td class="px-5 py-3 text-white">$${o.total.toFixed(2)}</td>
                </tr>
            `).join('');

            // Render Inventory Snapshot (Low Stock)
            const list = document.getElementById('admin-inventory-list');
            const lowStock = [...mockProducts].sort((a,b) => a.stock - b.stock).slice(0, 4);
            
            list.innerHTML = lowStock.map(p => `
                <div class="flex justify-between items-center bg-slate-900/50 p-3 rounded border border-slate-700/50">
                    <div class="flex items-center gap-3">
                        <img src="${p.img}" class="w-10 h-10 object-contain bg-slate-800 rounded p-1">
                        <div>
                            <div class="text-sm font-medium text-white line-clamp-1">${p.name}</div>
                            <div class="text-xs text-slate-500">ID: ${p.id}</div>
                        </div>
                    </div>
                    <div class="text-right">
                        <div class="text-sm font-bold ${p.stock < 10 ? 'text-brand-accent' : 'text-slate-300'}">${p.stock} units</div>
                        <div class="text-[10px] uppercase text-slate-500">In Stock</div>
                    </div>
                </div>
            `).join('');
        }

        function showToast(message, type = 'success') {
            const toast = document.getElementById('toast');
            const msgEl = document.getElementById('toast-msg');
            const iconEl = document.getElementById('toast-icon');
            
            msgEl.innerText = message;
            if(type === 'success') {
                iconEl.className = 'fas fa-check-circle text-green-400';
            } else {
                iconEl.className = 'fas fa-exclamation-circle text-red-400';
            }
            
            toast.classList.add('show');
            setTimeout(() => toast.classList.remove('show'), 3000);
        }

        // Initialize app on load
        window.addEventListener('DOMContentLoaded', init);

    </script>
</body>
</html>
```
