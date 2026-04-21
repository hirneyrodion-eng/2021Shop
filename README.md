# 2021Shop
import React, { useState, useEffect } from 'react';
import { motion, AnimatePresence } from 'framer-motion';
import { 
  ShoppingBag, 
  Menu, 
  X, 
  Plus, 
  ChevronRight, 
  Zap, 
  Instagram, 
  Github 
} from 'lucide-react';

// --- Mock Data ---
const PRODUCTS = [
  { id: 1, name: 'AJ1 HIGH "DIOR"', price: 8200, category: 'Archive', image: 'https://images.unsplash.com/photo-1597043530274-f899143397bd?auto=format&fit=crop&q=80&w=800' },
  { id: 2, name: 'NIKE DUNK LOW PANDA', price: 240, category: 'Best Sellers', image: 'https://images.unsplash.com/photo-1600185365483-26d7a4cc7519?auto=format&fit=crop&q=80&w=800' },
  { id: 3, name: 'GOTHIC OVERSIZE HOODIE', price: 120, category: 'New Arrivals', image: 'https://images.unsplash.com/photo-1556821840-3a63f95609a7?auto=format&fit=crop&q=80&w=800' },
  { id: 4, name: 'TECHWEAR SHELL JKT-01', price: 450, category: 'Archive', image: 'https://images.unsplash.com/photo-1551488831-00ddcb6c6bd3?auto=format&fit=crop&q=80&w=800' },
  { id: 5, name: 'UTILITY CARGO PANTS', price: 180, category: 'New Arrivals', image: 'https://images.unsplash.com/photo-1624378439575-d8705ad7ae80?auto=format&fit=crop&q=80&w=800' },
  { id: 6, name: 'ASICS GEL-KAYANO 14', price: 160, category: 'Best Sellers', image: 'https://images.unsplash.com/photo-1605348532760-6753d2c43329?auto=format&fit=crop&q=80&w=800' },
];

const ArchiveApp = () => {
  const [isCartOpen, setIsCartOpen] = useState(false);
  const [cart, setCart] = useState([]);
  const [filter, setFilter] = useState('All');

  const addToCart = (product) => {
    setCart([...cart, product]);
    setIsCartOpen(true);
  };

  const totalPrice = cart.reduce((acc, item) => acc + item.price, 0);

  return (
    <div className="min-h-screen bg-[#050505] text-white font-sans selection:bg-[#00f0ff] selection:text-black">
      
      {/* Navigation */}
      <nav className="fixed top-0 w-full z-50 border-b border-white/10 bg-black/50 backdrop-blur-xl">
        <div className="max-w-7xl mx-auto px-6 h-20 flex items-center justify-between">
          <div className="text-2xl font-black tracking-tighter italic">ARCHIVE-2021</div>
          
          <div className="hidden md:flex space-x-8 text-xs uppercase tracking-widest font-medium">
            {['Shop', 'Lookbook', 'Archive', 'Contact'].map(item => (
              <a key={item} href="#" className="hover:text-[#00f0ff] transition-colors">{item}</a>
            ))}
          </div>

          <div className="flex items-center space-x-6">
            <button onClick={() => setIsCartOpen(true)} className="relative p-2">
              <ShoppingBag size={20} />
              {cart.length > 0 && (
                <span className="absolute -top-1 -right-1 bg-[#00f0ff] text-black text-[10px] font-bold w-4 h-4 rounded-full flex items-center justify-center">
                  {cart.length}
                </span>
              )}
            </button>
            <Menu size={20} className="md:hidden" />
          </div>
        </div>
      </nav>

      {/* Hero Section */}
      <header className="relative pt-40 pb-20 px-6 overflow-hidden">
        <motion.div 
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          className="max-w-7xl mx-auto"
        >
          <h1 className="text-[12vw] leading-[0.85] font-black uppercase tracking-tighter mb-8">
            Drop <span className="text-transparent border-t-2 border-b-2 border-white">2021</span><br />
            <span className="text-[#00f0ff]">Archive</span> Street
          </h1>
          <div className="flex flex-wrap gap-4 mt-12">
            {['All', 'New Arrivals', 'Best Sellers', 'Archive'].map(tag => (
              <button
                key={tag}
                onClick={() => setFilter(tag)}
                className={`px-6 py-2 text-xs uppercase tracking-widest border transition-all duration-300 ${
                  filter === tag ? 'bg-white text-black border-white' : 'border-white/20 hover:border-[#00f0ff]'
                }`}
              >
                {tag}
              </button>
            ))}
          </div>
        </motion.div>
      </header>

      {/* Product Grid */}
      <main className="max-w-7xl mx-auto px-6 pb-40">
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-px bg-white/10 border border-white/10">
          {PRODUCTS.filter(p => filter === 'All' || p.category === filter).map((product) => (
            <motion.div 
              layout
              key={product.id}
              className="group relative bg-[#050505] p-6 flex flex-col"
            >
              <div className="aspect-[4/5] overflow-hidden bg-[#111] relative">
                <motion.img 
                  whileHover={{ scale: 1.05 }}
                  transition={{ duration: 0.6, ease: [0.33, 1, 0.68, 1] }}
                  src={product.image} 
                  alt={product.name}
                  className="w-full h-full object-cover grayscale hover:grayscale-0 transition-all duration-700"
                />
                <button 
                  onClick={() => addToCart(product)}
                  className="absolute bottom-4 left-4 right-4 bg-white text-black py-4 text-xs font-bold uppercase translate-y-12 group-hover:translate-y-0 transition-transform duration-300 flex items-center justify-center gap-2"
                >
                  <Plus size={16} /> Quick Add
                </button>
              </div>
              
              <div className="mt-6 flex justify-between items-start">
                <div>
                  <p className="text-[10px] text-[#00f0ff] uppercase tracking-widest mb-1">{product.category}</p>
                  <h3 className="text-lg font-bold tracking-tight uppercase">{product.name}</h3>
                </div>
                <p className="text-lg font-mono">${product.price}</p>
              </div>
            </motion.div>
          ))}
        </div>
      </main>

      {/* Cart Sidebar */}
      <AnimatePresence>
        {isCartOpen && (
          <>
            <motion.div 
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              exit={{ opacity: 0 }}
              onClick={() => setIsCartOpen(false)}
              className="fixed inset-0 bg-black/80 backdrop-blur-sm z-[60]"
            />
            <motion.div 
              initial={{ x: '100%' }}
              animate={{ x: 0 }}
              exit={{ x: '100%' }}
              transition={{ type: 'spring', damping: 25, stiffness: 200 }}
              className="fixed right-0 top-0 h-full w-full max-w-md bg-[#0a0a0a] border-l border-white/10 z-[70] p-8 flex flex-col"
            >
              <div className="flex justify-between items-center mb-12">
                <h2 className="text-2xl font-black uppercase tracking-tighter">Your Bag</h2>
                <button onClick={() => setIsCartOpen(false)}><X size={24} /></button>
              </div>

              <div className="flex-1 overflow-y-auto space-y-6">
                {cart.length === 0 ? (
                  <p className="text-white/40 uppercase tracking-widest text-xs">Cart is empty_</p>
                ) : (
                  cart.map((item, idx) => (
                    <div key={idx} className="flex gap-4 border-b border-white/5 pb-4">
                      <img src={item.image} className="w-20 h-24 object-cover grayscale" alt="" />
                      <div className="flex-1">
                        <h4 className="text-sm font-bold uppercase">{item.name}</h4>
                        <p className="text-[#00f0ff] font-mono mt-1">${item.price}</p>
                      </div>
                    </div>
                  ))
                )}
              </div>

              <div className="pt-8 border-t border-white/10">
                <div className="flex justify-between mb-6">
                  <span className="uppercase tracking-widest text-xs text-white/50">Total</span>
                  <span className="text-xl font-mono">${totalPrice}</span>
                </div>
                <button className="w-full bg-[#00f0ff] text-black py-5 text-xs font-black uppercase tracking-widest hover:bg-white transition-colors flex items-center justify-center gap-3">
                  Checkout Now <ChevronRight size={16} />
                </button>
              </div>
            </motion.div>
          </>
        )}
      </AnimatePresence>

      {/* Footer */}
      <footer className="border-t border-white/10 py-20 px-6">
        <div className="max-w-7xl mx-auto grid grid-cols-1 md:grid-cols-3 gap-12">
          <div>
            <div className="text-xl font-black mb-6 italic">ARCHIVE-2021</div>
            <p className="text-sm text-white/40 leading-relaxed max-w-xs">
              Curated brutalist streetwear and rare archives from the heart of the CIS underground.
            </p>
          </div>
          <div className="grid grid-cols-2 gap-8">
            <div className="space-y-4">
              <h4 className="text-[10px] uppercase tracking-[0.2em] text-white/30">Support</h4>
              <ul className="text-xs space-y-2 uppercase font-medium">
                <li><a href="#" className="hover:text-[#00f0ff]">Shipping</a></li>
                <li><a href="#" className="hover:text-[#00f0ff]">Returns</a></li>
              </ul>
            </div>
            <div className="space-y-4">
              <h4 className="text-[10px] uppercase tracking-[0.2em] text-white/30">Connect</h4>
              <div className="flex gap-4">
                <Instagram size={18} className="hover:text-[#00f0ff] cursor-pointer" />
                <Github size={18} className="hover:text-[#00f0ff] cursor-pointer" />
              </div>
            </div>
          </div>
          <div className="flex items-end justify-end">
            <div className="text-[10px] text-white/20 uppercase tracking-widest">
              © 2021 ARCHIVE WORLDWIDE LLC
            </div>
          </div>
        </div>
      </footer>
    </div>
  );
};

export default ArchiveApp;
