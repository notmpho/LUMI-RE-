# LUMI-RE-<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LUMIÈRE | Premium Cosmetics</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,600;0,700;1,400&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        :root {
            --primary: #FF6B6B;
            --secondary: #FFE66D;
            --accent: #4ECDC4;
            --dark: #2D3436;
            --light: #F7F1E3;
        }
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: #FAFAFA;
        }
        
        .font-serif {
            font-family: 'Playfair Display', serif;
        }
        
        .glass {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
        }
        
        .glass-dark {
            background: rgba(45, 52, 54, 0.95);
            backdrop-filter: blur(12px);
        }
        
        .gradient-text {
            background: linear-gradient(135deg, #FF6B6B 0%, #FFE66D 50%, #4ECDC4 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        
        .product-card {
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .product-card:hover {
            transform: translateY(-8px);
        }
        
        .product-card:hover .product-image {
            transform: scale(1.05);
        }
        
        .product-image {
            transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .cart-sidebar {
            transform: translateX(100%);
            transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .cart-sidebar.open {
            transform: translateX(0);
        }
        
        .overlay {
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
        }
        
        .overlay.active {
            opacity: 1;
            pointer-events: all;
        }
        
        .floating {
            animation: float 6s ease-in-out infinite;
        }
        
        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-20px); }
        }
        
        .shimmer {
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
            background-size: 200% 100%;
            animation: shimmer 2s infinite;
        }
        
        @keyframes shimmer {
            0% { background-position: -200% 0; }
            100% { background-position: 200% 0; }
        }
        
        .blob {
            position: absolute;
            filter: blur(80px);
            opacity: 0.6;
            animation: move 20s infinite alternate;
        }
        
        @keyframes move {
            from { transform: translate(0, 0) scale(1); }
            to { transform: translate(50px, -50px) scale(1.1); }
        }
        
        .category-pill {
            transition: all 0.3s ease;
        }
        
        .category-pill.active {
            background: linear-gradient(135deg, #FF6B6B, #FF8E53);
            color: white;
            transform: scale(1.05);
        }
        
        .notification {
            transform: translateY(-100%);
            transition: transform 0.3s ease;
        }
        
        .notification.show {
            transform: translateY(0);
        }
        
        .scroll-reveal {
            opacity: 0;
            transform: translateY(30px);
            transition: all 0.8s ease;
        }
        
        .scroll-reveal.visible {
            opacity: 1;
            transform: translateY(0);
        }
    </style>
</head>
<body class="text-gray-800 overflow-x-hidden">

    <!-- Notification Toast -->
    <div id="notification" class="notification fixed top-0 left-0 right-0 z-50 bg-green-500 text-white text-center py-3 font-medium">
        Item added to cart!
    </div>

    <!-- Overlay -->
    <div id="overlay" class="overlay fixed inset-0 bg-black/50 z-40" onclick="toggleCart()"></div>

    <!-- Navigation -->
    <nav class="fixed w-full z-30 glass border-b border-white/20">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between items-center h-20">
                <div class="flex items-center cursor-pointer" onclick="window.scrollTo({top: 0, behavior: 'smooth'})">
                    <span class="font-serif text-3xl font-bold gradient-text">LUMIÈRE</span>
                </div>
                
                <div class="hidden md:flex items-center space-x-8">
                    <a href="#products" class="text-gray-700 hover:text-pink-500 transition-colors font-medium">Shop</a>
                    <a href="#categories" class="text-gray-700 hover:text-pink-500 transition-colors font-medium">Categories</a>
                    <a href="#about" class="text-gray-700 hover:text-pink-500 transition-colors font-medium">About</a>
                </div>

                <div class="flex items-center space-x-4">
                    <button class="p-2 hover:bg-gray-100 rounded-full transition-colors">
                        <i data-lucide="search" class="w-5 h-5 text-gray-700"></i>
                    </button>
                    <button class="p-2 hover:bg-gray-100 rounded-full transition-colors">
                        <i data-lucide="user" class="w-5 h-5 text-gray-700"></i>
                    </button>
                    <button onclick="toggleCart()" class="relative p-2 hover:bg-gray-100 rounded-full transition-colors">
                        <i data-lucide="shopping-bag" class="w-5 h-5 text-gray-700"></i>
                        <span id="cart-count" class="absolute -top-1 -right-1 bg-gradient-to-r from-pink-500 to-orange-400 text-white text-xs w-5 h-5 rounded-full flex items-center justify-center font-bold">0</span>
                    </button>
                </div>
            </div>
        </div>
    </nav>

    <!-- Cart Sidebar -->
    <div id="cart-sidebar" class="cart-sidebar fixed right-0 top-0 h-full w-full sm:w-96 glass-dark z-50 text-white overflow-y-auto">
        <div class="p-6">
            <div class="flex justify-between items-center mb-8">
                <h2 class="font-serif text-2xl font-bold">Your Cart</h2>
                <button onclick="toggleCart()" class="p-2 hover:bg-white/10 rounded-full transition-colors">
                    <i data-lucide="x" class="w-6 h-6"></i>
                </button>
            </div>
            
            <div id="cart-items" class="space-y-4 mb-8">
                <!-- Cart items will be inserted here -->
            </div>
            
            <div id="cart-empty" class="text-center py-12 text-gray-400">
                <i data-lucide="shopping-bag" class="w-16 h-16 mx-auto mb-4 opacity-50"></i>
                <p>Your cart is empty</p>
            </div>

            <div id="cart-footer" class="hidden border-t border-white/20 pt-6">
                <div class="flex justify-between mb-4 text-lg">
                    <span>Subtotal</span>
                    <span id="cart-total" class="font-bold">$0.00</span>
                </div>
                <button onclick="checkout()" class="w-full bg-gradient-to-r from-pink-500 to-orange-400 text-white py-4 rounded-full font-semibold hover:shadow-lg hover:scale-105 transition-all">
                    Checkout
                </button>
                <p class="text-center text-sm text-gray-400 mt-4">Free shipping on orders over $50</p>
            </div>
        </div>
    </div>

    <!-- Hero Section -->
    <section class="relative min-h-screen flex items-center justify-center overflow-hidden pt-20">
        <!-- Animated Background Blobs -->
        <div class="blob bg-pink-300 w-96 h-96 rounded-full top-20 -left-20"></div>
        <div class="blob bg-yellow-200 w-80 h-80 rounded-full bottom-20 right-0 delay-1000"></div>
        <div class="blob bg-teal-200 w-64 h-64 rounded-full top-1/2 left-1/2 delay-2000"></div>

        <div class="relative z-10 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 grid lg:grid-cols-2 gap-12 items-center">
            <div class="text-center lg:text-left scroll-reveal">
                <span class="inline-block px-4 py-2 rounded-full bg-white/80 backdrop-blur-sm text-pink-600 font-semibold text-sm mb-6 border border-pink-100">
                    ✨ New Collection 2026
                </span>
                <h1 class="font-serif text-5xl lg:text-7xl font-bold text-gray-900 leading-tight mb-6">
                    Unleash Your <br>
                    <span class="gradient-text italic">Inner Glow</span>
                </h1>
                <p class="text-xl text-gray-600 mb-8 max-w-lg mx-auto lg:mx-0 leading-relaxed">
                    Discover premium cruelty-free cosmetics crafted to enhance your natural beauty. From vibrant lipsticks to radiant foundations.
                </p>
                <div class="flex flex-col sm:flex-row gap-4 justify-center lg:justify-start">
                    <button onclick="document.getElementById('products').scrollIntoView({behavior: 'smooth'})" class="px-8 py-4 bg-gradient-to-r from-pink-500 to-orange-400 text-white rounded-full font-semibold hover:shadow-xl hover:scale-105 transition-all flex items-center justify-center gap-2">
                        Shop Now <i data-lucide="arrow-right" class="w-5 h-5"></i>
                    </button>
                    <button class="px-8 py-4 bg-white text-gray-800 rounded-full font-semibold hover:shadow-xl hover:scale-105 transition-all border border-gray-200">
                        View Lookbook
                    </button>
                </div>
                
                <div class="mt-12 flex items-center justify-center lg:justify-start gap-8">
                    <div class="text-center">
                        <p class="font-serif text-3xl font-bold text-gray-900">50K+</p>
                        <p class="text-sm text-gray-500">Happy Customers</p>
                    </div>
                    <div class="w-px h-12 bg-gray-300"></div>
                    <div class="text-center">
                        <p class="font-serif text-3xl font-bold text-gray-900">100%</p>
                        <p class="text-sm text-gray-500">Cruelty Free</p>
                    </div>
                    <div class="w-px h-12 bg-gray-300"></div>
                    <div class="text-center">
                        <p class="font-serif text-3xl font-bold text-gray-900">4.9</p>
                        <p class="text-sm text-gray-500">Rating</p>
                    </div>
                </div>
            </div>
            
            <div class="relative scroll-reveal delay-200">
                <div class="floating relative z-10">
                    <img src="https://images.unsplash.com/photo-1596462502278-27bfdc403348?w=800&auto=format&fit=crop&q=80" 
                         alt="Cosmetics Collection" 
                         class="rounded-3xl shadow-2xl w-full object-cover h-[600px]">
                    
                    <!-- Floating Product Cards -->
                    <div class="absolute -left-8 top-1/4 glass p-4 rounded-2xl shadow-xl animate-pulse">
                        <div class="flex items-center gap-3">
                            <img src="https://images.unsplash.com/photo-1586495777744-4413f21062fa?w=100&auto=format&fit=crop&q=80" class="w-12 h-12 rounded-full object-cover">
                            <div>
                                <p class="font-semibold text-sm">Rose Lipstick</p>
                                <p class="text-pink-500 font-bold">$24</p>
                            </div>
                        </div>
                    </div>
                    
                    <div class="absolute -right-4 bottom-1/4 glass p-4 rounded-2xl shadow-xl animate-pulse delay-700">
                        <div class="flex items-center gap-3">
                            <img src="https://images.unsplash.com/photo-1612817288484-6f916006741a?w=100&auto=format&fit=crop&q=80" class="w-12 h-12 rounded-full object-cover">
                            <div>
                                <p class="font-semibold text-sm">Glow Serum</p>
                                <p class="text-pink-500 font-bold">$45</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Categories -->
    <section id="categories" class="py-20 bg-white">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="text-center mb-12 scroll-reveal">
                <h2 class="font-serif text-4xl font-bold text-gray-900 mb-4">Shop by Category</h2>
                <p class="text-gray-600">Find exactly what you're looking for</p>
            </div>
            
            <div class="flex flex-wrap justify-center gap-4 mb-12 scroll-reveal">
                <button onclick="filterProducts('all')" class="category-pill active px-6 py-3 rounded-full border border-gray-200 font-medium hover:border-pink-300" data-category="all">All Products</button>
                <button onclick="filterProducts('face')" class="category-pill px-6 py-3 rounded-full border border-gray-200 font-medium hover:border-pink-300" data-category="face">Face</button>
                <button onclick="filterProducts('eyes')" class="category-pill px-6 py-3 rounded-full border border-gray-200 font-medium hover:border-pink-300" data-category="eyes">Eyes</button>
                <button onclick="filterProducts('lips')" class="category-pill px-6 py-3 rounded-full border border-gray-200 font-medium hover:border-pink-300" data-category="lips">Lips</button>
                <button onclick="filterProducts('skincare')" class="category-pill px-6 py-3 rounded-full border border-gray-200 font-medium hover:border-pink-300" data-category="skincare">Skincare</button>
            </div>
        </div>
    </section>

    <!-- Products Grid -->
    <section id="products" class="py-20 bg-gray-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between items-end mb-12 scroll-reveal">
                <div>
                    <h2 class="font-serif text-4xl font-bold text-gray-900 mb-2">Best Sellers</h2>
                    <p class="text-gray-600">Handpicked favorites from our collection</p>
                </div>
                <div class="hidden sm:flex gap-2">
                    <button class="p-2 rounded-full border border-gray-300 hover:border-pink-500 hover:text-pink-500 transition-colors">
                        <i data-lucide="chevron-left" class="w-5 h-5"></i>
                    </button>
                    <button class="p-2 rounded-full border border-gray-300 hover:border-pink-500 hover:text-pink-500 transition-colors">
                        <i data-lucide="chevron-right" class="w-5 h-5"></i>
                    </button>
                </div>
            </div>

            <div id="products-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8">
                <!-- Products will be rendered here -->
            </div>
        </div>
    </section>

    <!-- Featured Banner -->
    <section class="py-20 relative overflow-hidden">
        <div class="absolute inset-0 bg-gradient-to-r from-pink-500 to-orange-400"></div>
        <div class="absolute inset-0 opacity-20">
            <div class="absolute inset-0" style="background-image: radial-gradient(circle at 2px 2px, white 1px, transparent 0); background-size: 40px 40px;"></div>
        </div>
        
        <div class="relative z-10 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center text-white">
            <h2 class="font-serif text-5xl font-bold mb-6 scroll-reveal">Summer Glow Collection</h2>
            <p class="text-xl mb-8 max-w-2xl mx-auto opacity-90 scroll-reveal">Get 25% off our new bronzing collection. Limited time offer for that perfect sun-kissed look.</p>
            <button class="px-8 py-4 bg-white text-pink-500 rounded-full font-bold hover:shadow-xl hover:scale-105 transition-all scroll-reveal">
                Shop Collection
            </button>
        </div>
    </section>

    <!-- Features -->
    <section class="py-20 bg-white">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="grid md:grid-cols-3 gap-12">
                <div class="text-center scroll-reveal">
                    <div class="w-16 h-16 mx-auto mb-6 bg-pink-100 rounded-full flex items-center justify-center">
                        <i data-lucide="truck" class="w-8 h-8 text-pink-500"></i>
                    </div>
                    <h3 class="font-serif text-xl font-bold mb-2">Free Shipping</h3>
                    <p class="text-gray-600">On all orders over $50. Express delivery available.</p>
                </div>
                <div class="text-center scroll-reveal delay-100">
                    <div class="w-16 h-16 mx-auto mb-6 bg-teal-100 rounded-full flex items-center justify-center">
                        <i data-lucide="leaf" class="w-8 h-8 text-teal-500"></i>
                    </div>
                    <h3 class="font-serif text-xl font-bold mb-2">100% Vegan</h3>
                    <p class="text-gray-600">All products are cruelty-free and ethically sourced.</p>
                </div>
                <div class="text-center scroll-reveal delay-200">
                    <div class="w-16 h-16 mx-auto mb-6 bg-orange-100 rounded-full flex items-center justify-center">
                        <i data-lucide="refresh-cw" class="w-8 h-8 text-orange-500"></i>
                    </div>
                    <h3 class="font-serif text-xl font-bold mb-2">Easy Returns</h3>
                    <p class="text-gray-600">30-day money back guarantee on all purchases.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Testimonials -->
    <section class="py-20 bg-gray-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <h2 class="font-serif text-4xl font-bold text-center mb-16 scroll-reveal">What Our Customers Say</h2>
            
            <div class="grid md:grid-cols-3 gap-8">
                <div class="bg-white p-8 rounded-2xl shadow-lg scroll-reveal">
                    <div class="flex text-yellow-400 mb-4">
                        <i data-lucide="star" class="w-5 h-5 fill-current"></i>
                        <i data-lucide="star" class="w-5 h-5 fill-current"></i>
                        <i data-lucide="star" class="w-5 h-5 fill-current"></i>
                        <i data-lucide="star" class="w-5 h-5 fill-current"></i>
                        <i data-lucide="star" class="w-5 h-5 fill-current"></i>
                    </div>
                    <p class="text-gray-600 mb-6">"The quality of these cosmetics is incredible. The lipstick stays on all day and the colors are so vibrant!"</p>
                    <div class="flex items-center gap-4">
                        <img src="https://images.unsplash.com/photo-1494790108377-be9c29b29330?w=100&auto=format&fit=crop&q=80" class="w-12 h-12 rounded-full object-cover">
                        <div>
                            <p class="font-semibold">Sarah Johnson</p>
                            <p class="text-sm text-gray-500">Verified Buyer</p>
                        </div>
                    </div>
                </div>
                
                <div class="bg-white p-8 rounded-2xl shadow-lg scroll-reveal delay-100">
                    <div class="flex text-yellow-400 mb-4">
                        <i data-lucide="star" class="w-5 h-5 fill-current"></i>
                        <i data-lucide="star" class="w-5 h-5 fill-current"></i>
                        <i data-lucide="star" class="w-5 h-5 fill-current"></i>
                        <i data-lucide="star" class="w-5 h-5 fill-current"></i>
                        <i data-lucide="star" class="w-5 h-5 fill-current"></i>
                    </div>
                    <p class="text-gray-600 mb-6">"Finally found a foundation that matches my skin perfectly. The coverage is buildable and feels so lightweight."</p>
                    <div class="flex items-center gap-4">
                        <img src="https://images.unsplash.com/photo-1438761681033-6461ffad8d80?w=100&auto=format&fit=crop&q=80" class="w-12 h-12 rounded-full object-cover">
                        <div>
                            <p class="font-semibold">Emily Chen</p>
                            <p class="text-sm text-gray-500">Verified Buyer</p>
                        </div>
                    </div>
                </div>
                
                <div class="bg-white p-8 rounded-2xl shadow-lg scroll-reveal delay-200">
                    <div class="flex text-yellow-400 mb-4">
                        <i data-lucide="star" class="w-5 h-5 fill-current"></i>
                        <i data-lucide="star" class="w-5 h-5 fill-current"></i>
                        <i data-lucide="star" class="w-5 h-5 fill-current"></i>
                        <i data-lucide="star" class="w-5 h-5 fill-current"></i>
                        <i data-lucide="star" class="w-5 h-5 fill-current"></i>
                    </div>
                    <p class="text-gray-600 mb-6">"Love that everything is vegan and cruelty-free. The skincare line has completely transformed my routine."</p>
                    <div class="flex items-center gap-4">
                        <img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=100&auto=format&fit=crop&q=80" class="w-12 h-12 rounded-full object-cover">
                        <div>
                            <p class="font-semibold">Maria Garcia</p>
                            <p class="text-sm text-gray-500">Verified Buyer</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Newsletter -->
    <section class="py-20 bg-gray-900 text-white">
        <div class="max-w-4xl mx-auto px-4 text-center">
            <h2 class="font-serif text-4xl font-bold mb-4 scroll-reveal">Join the Glow Club</h2>
            <p class="text-gray-400 mb-8 scroll-reveal">Subscribe for exclusive offers, beauty tips, and early access to new collections.</p>
            
            <form class="flex flex-col sm:flex-row gap-4 max-w-lg mx-auto scroll-reveal" onsubmit="event.preventDefault(); alert('Welcome to the Glow Club!');">
                <input type="email" placeholder="Enter your email" class="flex-1 px-6 py-4 rounded-full bg-white/10 border border-white/20 text-white placeholder-gray-400 focus:outline-none focus:border-pink-500">
                <button type="submit" class="px-8 py-4 bg-gradient-to-r from-pink-500 to-orange-400 rounded-full font-semibold hover:shadow-lg hover:scale-105 transition-all">
                    Subscribe
                </button>
            </form>
            
            <p class="text-sm text-gray-500 mt-4">By subscribing, you agree to our Privacy Policy.</p>
        </div>
    </section>

    <!-- Footer -->
    <footer class="bg-gray-50 border-t border-gray-200 pt-16 pb-8">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="grid md:grid-cols-4 gap-8 mb-12">
                <div>
                    <span class="font-serif text-2xl font-bold gradient-text">LUMIÈRE</span>
                    <p class="mt-4 text-gray-600 text-sm leading-relaxed">Premium cosmetics for the modern woman. Cruelty-free, vegan, and sustainably sourced.</p>
                    <div class="flex gap-4 mt-6">
                        <a href="#" class="w-10 h-10 rounded-full bg-white border border-gray-200 flex items-center justify-center hover:border-pink-500 hover:text-pink-500 transition-colors">
                            <i data-lucide="instagram" class="w-5 h-5"></i>
                        </a>
                        <a href="#" class="w-10 h-10 rounded-full bg-white border border-gray-200 flex items-center justify-center hover:border-pink-500 hover:text-pink-500 transition-colors">
                            <i data-lucide="facebook" class="w-5 h-5"></i>
                        </a>
                        <a href="#" class="w-10 h-10 rounded-full bg-white border border-gray-200 flex items-center justify-center hover:border-pink-500 hover:text-pink-500 transition-colors">
                            <i data-lucide="twitter" class="w-5 h-5"></i>
                        </a>
                    </div>
                </div>
                
                <div>
                    <h4 class="font-bold mb-4">Shop</h4>
                    <ul class="space-y-2 text-sm text-gray-600">
                        <li><a href="#" class="hover:text-pink-500 transition-colors">New Arrivals</a></li>
                        <li><a href="#" class="hover:text-pink-500 transition-colors">Best Sellers</a></li>
                        <li><a href="#" class="hover:text-pink-500 transition-colors">Face</a></li>
                        <li><a href="#" class="hover:text-pink-500 transition-colors">Eyes</a></li>
                        <li><a href="#" class="hover:text-pink-500 transition-colors">Lips</a></li>
                    </ul>
                </div>
                
                <div>
                    <h4 class="font-bold mb-4">Help</h4>
                    <ul class="space-y-2 text-sm text-gray-600">
                        <li><a href="#" class="hover:text-pink-500 transition-colors">Shipping Info</a></li>
                        <li><a href="#" class="hover:text-pink-500 transition-colors">Returns</a></li>
                        <li><a href="#" class="hover:text-pink-500 transition-colors">FAQ</a></li>
                        <li><a href="#" class="hover:text-pink-500 transition-colors">Contact Us</a></li>
                    </ul>
                </div>
                
                <div>
                    <h4 class="font-bold mb-4">Contact</h4>
                    <ul class="space-y-2 text-sm text-gray-600">
                        <li class="flex items-center gap-2">
                            <i data-lucide="mail" class="w-4 h-4"></i>
                            hello@lumiere.com
                        </li>
                        <li class="flex items-center gap-2">
                            <i data-lucide="phone" class="w-4 h-4"></i>
                            1-800-LUMIERE
                        </li>
                        <li class="flex items-center gap-2">
                            <i data-lucide="map-pin" class="w-4 h-4"></i>
                            New York, NY
                        </li>
                    </ul>
                </div>
            </div>
            
            <div class="border-t border-gray-200 pt-8 flex flex-col md:flex-row justify-between items-center gap-4">
                <p class="text-sm text-gray-500">&copy; 2026 LUMIÈRE Cosmetics. All rights reserved.</p>
                <div class="flex gap-6 text-sm text-gray-500">
                    <a href="#" class="hover:text-pink-500 transition-colors">Privacy Policy</a>
                    <a href="#" class="hover:text-pink-500 transition-colors">Terms of Service</a>
                </div>
            </div>
        </div>
    </footer>

    <script>
        // Initialize Lucide icons
        lucide.createIcons();

        // Product Data
        const products = [
            {
                id: 1,
                name: "Velvet Matte Lipstick",
                category: "lips",
                price: 24,
                image: "https://images.unsplash.com/photo-1586495777744-4413f21062fa?w=600&auto=format&fit=crop&q=80",
                badge: "Best Seller",
                rating: 4.9
            },
            {
                id: 2,
                name: "Radiance Foundation",
                category: "face",
                price: 42,
                image: "https://images.unsplash.com/photo-1612817288484-6f916006741a?w=600&auto=format&fit=crop&q=80",
                badge: "New",
                rating: 4.8
            },
            {
                id: 3,
                name: "Rose Gold Eyeshadow Palette",
                category: "eyes",
                price: 38,
                image: "https://images.unsplash.com/photo-1596462502278-27bfdc403348?w=600&auto=format&fit=crop&q=80",
                badge: null,
                rating: 4.7
            },
            {
                id: 4,
                name: "Hydrating Glow Serum",
                category: "skincare",
                price: 56,
                image: "https://images.unsplash.com/photo-1620916566398-39f1143ab7be?w=600&auto=format&fit=crop&q=80",
                badge: "Popular",
                rating: 4.9
            },
            {
                id: 5,
                name: "Volumizing Mascara",
                category: "eyes",
                price: 28,
                image: "https://images.unsplash.com/photo-1631214524115-6f8eb1beb6ae?w=600&auto=format&fit=crop&q=80",
                badge: null,
                rating: 4.6
            },
            {
                id: 6,
                name: "Silk Pressed Powder",
                category: "face",
                price: 34,
                image: "https://images.unsplash.com/photo-1616683693504-3ea7e9ad6fec?w=600&auto=format&fit=crop&q=80",
                badge: null,
                rating: 4.5
            },
            {
                id: 7,
                name: "Lip Gloss Set",
                category: "lips",
                price: 32,
                image: "https://images.unsplash.com/photo-1608248543803-ba4f8c70ae0b?w=600&auto=format&fit=crop&q=80",
                badge: "Sale",
                rating: 4.8
            },
            {
                id: 8,
                name: "Vitamin C Brightening Cream",
                category: "skincare",
                price: 48,
                image: "https://images.unsplash.com/photo-1556228720-195a672e8a03?w=600&auto=format&fit=crop&q=80",
                badge: null,
                rating: 4.7
            }
        ];

        let cart = [];
        let currentCategory = 'all';

        // Render Products
        function renderProducts() {
            const grid = document.getElementById('products-grid');
            const filtered = currentCategory === 'all' 
                ? products 
                : products.filter(p => p.category === currentCategory);
            
            grid.innerHTML = filtered.map(product => `
                <div class="product-card bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-2xl group cursor-pointer scroll-reveal">
                    <div class="relative overflow-hidden aspect-square">
                        <img src="${product.image}" alt="${product.name}" class="product-image w-full h-full object-cover">
                        ${product.badge ? `
                            <span class="absolute top-4 left-4 px-3 py-1 bg-gradient-to-r from-pink-500 to-orange-400 text-white text-xs font-bold rounded-full">
                                ${product.badge}
                            </span>
                        ` : ''}
                        <button onclick="addToCart(${product.id})" class="absolute bottom-4 right-4 w-12 h-12 bg-white rounded-full shadow-lg flex items-center justify-center text-gray-800 hover:bg-pink-500 hover:text-white transition-all transform translate-y-16 group-hover:translate-y-0">
                            <i data-lucide="plus" class="w-6 h-6"></i>
                        </button>
                        <div class="absolute inset-0 bg-black/0 group-hover:bg-black/10 transition-colors"></div>
                    </div>
                    <div class="p-6">
                        <div class="flex items-center gap-1 mb-2">
                            <i data-lucide="star" class="w-4 h-4 text-yellow-400 fill-current"></i>
                            <span class="text-sm text-gray-600">${product.rating}</span>
                        </div>
                        <h3 class="font-serif text-lg font-bold text-gray-900 mb-1">${product.name}</h3>
                        <p class="text-gray-500 text-sm capitalize mb-3">${product.category}</p>
                        <div class="flex items-center justify-between">
                            <span class="text-xl font-bold text-gray-900">$${product.price}</span>
                            <button onclick="addToCart(${product.id})" class="text-sm font-semibold text-pink-500 hover:text-pink-600 transition-colors">
                                Add to Cart
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');
            
            lucide.createIcons();
            
            // Re-trigger scroll reveal for new elements
            setTimeout(() => {
                document.querySelectorAll('.scroll-reveal').forEach(el => observer.observe(el));
            }, 100);
        }

        // Filter Products
        function filterProducts(category) {
            currentCategory = category;
            
            // Update active pill
            document.querySelectorAll('.category-pill').forEach(pill => {
                if (pill.dataset.category === category) {
                    pill.classList.add('active');
                } else {
                    pill.classList.remove('active');
                }
            });
            
            renderProducts();
        }

        // Cart Functions
        function toggleCart() {
            const sidebar = document.getElementById('cart-sidebar');
            const overlay = document.getElementById('overlay');
            sidebar.classList.toggle('open');
            overlay.classList.toggle('active');
            document.body.style.overflow = sidebar.classList.contains('open') ? 'hidden' : '';
        }

        function addToCart(productId) {
            const product = products.find(p => p.id === productId);
            const existingItem = cart.find(item => item.id === productId);
            
            if (existingItem) {
                existingItem.quantity++;
            } else {
                cart.push({ ...product, quantity: 1 });
            }
            
            updateCart();
            showNotification();
            
            // Open cart automatically on first add
            if (cart.length === 1 && cart[0].quantity === 1) {
                toggleCart();
            }
        }

        function removeFromCart(productId) {
            cart = cart.filter(item => item.id !== productId);
            updateCart();
        }

        function updateQuantity(productId, delta) {
            const item = cart.find(item => item.id === productId);
            if (item) {
                item.quantity += delta;
                if (item.quantity <= 0) {
                    removeFromCart(productId);
                } else {
                    updateCart();
                }
            }
        }

        function updateCart() {
            const cartItems = document.getElementById('cart-items');
            const cartEmpty = document.getElementById('cart-empty');
            const cartFooter = document.getElementById('cart-footer');
            const cartCount = document.getElementById('cart-count');
            const cartTotal = document.getElementById('cart-total');
            
            const totalItems = cart.reduce((sum, item) => sum + item.quantity, 0);
            cartCount.textContent = totalItems;
            
            if (cart.length === 0) {
                cartItems.innerHTML = '';
                cartEmpty.classList.remove('hidden');
                cartFooter.classList.add('hidden');
            } else {
                cartEmpty.classList.add('hidden');
                cartFooter.classList.remove('hidden');
                
                cartItems.innerHTML = cart.map(item => `
                    <div class="flex gap-4 bg-white/5 p-4 rounded-xl">
                        <img src="${item.image}" class="w-20 h-20 object-cover rounded-lg">
                        <div class="flex-1">
                            <h4 class="font-semibold text-sm mb-1">${item.name}</h4>
                            <p class="text-pink-400 font-bold mb-2">$${item.price}</p>
                            <div class="flex items-center gap-3">
                                <button onclick="updateQuantity(${item.id}, -1)" class="w-6 h-6 rounded-full bg-white/10 flex items-center justify-center hover:bg-white/20 transition-colors">
                                    <i data-lucide="minus" class="w-3 h-3"></i>
                                </button>
                                <span class="text-sm font-medium">${item.quantity}</span>
                                <button onclick="updateQuantity(${item.id}, 1)" class="w-6 h-6 rounded-full bg-white/10 flex items-center justify-center hover:bg-white/20 transition-colors">
                                    <i data-lucide="plus" class="w-3 h-3"></i>
                                </button>
                            </div>
                        </div>
                        <button onclick="removeFromCart(${item.id})" class="text-gray-400 hover:text-white transition-colors">
                            <i data-lucide="trash-2" class="w-4 h-4"></i>
                        </button>
                    </div>
                `).join('');
                
                const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
                cartTotal.textContent = `$${total.toFixed(2)}`;
                
                lucide.createIcons();
            }
        }

        function showNotification() {
            const notification = document.getElementById('notification');
            notification.classList.add('show');
            setTimeout(() => {
                notification.classList.remove('show');
            }, 2000);
        }

        function checkout() {
            if (cart.length === 0) return;
            alert('Proceeding to checkout!\n\nThis is a demo dropshipping store.\nIn production, this would connect to Shopify/WooCommerce or your preferred payment processor.');
        }

        // Scroll Reveal Animation
        const observerOptions = {
            threshold: 0.1,
            rootMargin: '0px 0px -50px 0px'
        };

        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('visible');
                }
            });
        }, observerOptions);

        // Initialize
        document.addEventListener('DOMContentLoaded', () => {
            renderProducts();
            
            // Observe scroll reveal elements
            document.querySelectorAll('.scroll-reveal').forEach(el => {
                observer.observe(el);
            });
            
            // Smooth scroll for anchor links
            document.querySelectorAll('a[href^="#"]').forEach(anchor => {
                anchor.addEventListener('click', function (e) {
                    e.preventDefault();
                    const target = document.querySelector(this.getAttribute('href'));
                    if (target) {
                        target.scrollIntoView({ behavior: 'smooth', block: 'start' });
                    }
                });
            });
        });

        // Header scroll effect
        let lastScroll = 0;
        window.addEventListener('scroll', () => {
            const nav = document.querySelector('nav');
            const currentScroll = window.pageYOffset;
            
            if (currentScroll > 100) {
                nav.classList.add('shadow-md');
            } else {
                nav.classList.remove('shadow-md');
            }
            
            lastScroll = currentScroll;
        });
    </script>
</body>
</html>
