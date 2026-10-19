// --- TAILWIND CONFIGURATION ---
tailwind.config = {
    darkMode: 'class',
    theme: {
        extend: {
            colors: {
                brand: {
                    dark: '#0b0f19',
                    card: '#151c2c',
                    border: '#2a364f',
                    cyan: '#06b6d4',
                    purple: '#8b5cf6',
                    accent: '#3b82f6',
                    neon: '#00f0ff'
                }
            },
            fontFamily: {
                sans: ['Inter', 'sans-serif'],
                display: ['Orbitron', 'sans-serif']
            }
        }
    }
};

// --- GAME DATABASE ---
const gamesData = [
    {
        id: 'mlbb',
        title: 'Mobile Legends',
        publisher: 'Moonton',
        icon: 'fa-gamepad',
        bgGradient: 'from-blue-600 to-cyan-500',
        fields: [
            { id: 'userId', label: 'User ID', placeholder: 'Contoh: 12345678', type: 'text' },
            { id: 'zoneId', label: 'Zone ID', placeholder: 'Contoh: (1234)', type: 'text' }
        ],
        nominals: [
            { id: 1, name: '3 Diamonds', price: 1500 },
            { id: 2, name: '12 Diamonds', price: 3500 },
            { id: 3, name: '19 Diamonds', price: 6000 },
            { id: 4, name: '86 Diamonds', price: 20000 },
            { id: 5, name: '172 Diamonds', price: 40000 },
            { id: 6, name: 'Starlight Member', price: 75000 }
        ]
    },
    {
        id: 'ff',
        title: 'Free Fire',
        publisher: 'Garena',
        icon: 'fa-fire-flame-curved',
        bgGradient: 'from-orange-600 to-red-600',
        fields: [
            { id: 'userId', label: 'Player ID (UID)', placeholder: 'Contoh: 123456789', type: 'text' }
        ],
        nominals: [
            { id: 1, name: '50 Diamonds', price: 7000 },
            { id: 2, name: '70 Diamonds', price: 10000 },
            { id: 3, name: '140 Diamonds', price: 19000 },
            { id: 4, name: '355 Diamonds', price: 47000 },
            { id: 5, name: '720 Diamonds', price: 95000 },
            { id: 6, name: 'Membership Bulanan', price: 290000 }
        ]
    },
    {
        id: 'pubg',
        title: 'PUBG Mobile',
        publisher: 'Tencent Games',
        icon: 'fa-plane-departure',
        bgGradient: 'from-amber-600 to-yellow-500',
        fields: [
            { id: 'userId', label: 'Character ID', placeholder: 'Contoh: 512345678', type: 'text' }
        ],
        nominals: [
            { id: 1, name: '60 UC', price: 15000 },
            { id: 2, name: '325 UC', price: 75000 },
            { id: 3, name: '660 UC', price: 150000 },
            { id: 4, name: '1800 UC', price: 380000 }
        ]
    }
];

// --- APP STATE ---
let selectedGame = null;
let selectedNominal = null;
let selectedPayment = null;
let currentSlide = 0;

// --- INITIALIZATION ---
document.addEventListener('DOMContentLoaded', () => {
    renderGameGrid(gamesData);
    startCarouselAutoPlay();
});

// --- UI TAB SWITCHING ---
function switchTab(tabName) {
    document.querySelectorAll('.tab-content').forEach(tab => tab.classList.add('hidden'));
    const activeTab = document.getElementById(`tab-${tabName}`);
    if (activeTab) activeTab.classList.remove('hidden');
    window.scrollTo({ top: 0, behavior: 'smooth' });
}

function toggleMobileMenu() {
    const menu = document.getElementById('mobileMenu');
    menu.classList.toggle('hidden');
}

// --- RENDER GAME GRID ---
function renderGameGrid(data) {
    const grid = document.getElementById('gameGrid');
    if (!grid) return;
    
    grid.innerHTML = data.map(game => `
        <div onclick="openTopupModal('${game.id}')" class="bg-brand-card border border-brand-border hover:border-cyan-500/50 rounded-2xl p-4 card-hover cursor-pointer flex flex-col items-center text-center group relative overflow-hidden">
            <div class="w-16 h-16 sm:w-20 sm:h-20 rounded-2xl bg-gradient-to-tr ${game.bgGradient} flex items-center justify-center text-3xl text-white shadow-lg mb-3 transform group-hover:scale-110 transition duration-300">
                <i class="fa-solid ${game.icon}"></i>
            </div>
            <h4 class="text-sm font-bold text-white group-hover:text-cyan-400 transition line-clamp-1">${game.title}</h4>
            <p class="text-[11px] text-gray-400 mt-0.5">${game.publisher}</p>
        </div>
    `).join('');
}

function filterGames(query) {
    const filtered = gamesData.filter(g => 
        g.title.toLowerCase().includes(query.toLowerCase()) || 
        g.publisher.toLowerCase().includes(query.toLowerCase())
    );
    renderGameGrid(filtered);
}

// --- CAROUSEL LOGIC ---
function showSlide(index) {
    const slides = document.querySelectorAll('.carousel-item');
    if (slides.length === 0) return;
    
    if (index >= slides.length) currentSlide = 0;
    else if (index < 0) currentSlide = slides.length - 1;
    else currentSlide = index;

    slides.forEach((slide, i) => {
        if (i === currentSlide) {
            slide.classList.remove('opacity-0');
            slide.classList.add('opacity-100');
        } else {
            slide.classList.remove('opacity-100');
            slide.classList.add('opacity-0');
        }
    });
}

function nextSlide() { showSlide(currentSlide + 1); }
function prevSlide() { showSlide(currentSlide - 1); }

function startCarouselAutoPlay() {
    setInterval(() => nextSlide(), 5000);
}

// --- TOPUP MODAL LOGIC ---
function openTopupModal(gameId) {
    selectedGame = gamesData.find(g => g.id === gameId);
    if (!selectedGame) return;

    selectedNominal = null;
    selectedPayment = null;
    updateTotalPrice();

    // Render Title & Icon
    document.getElementById('modalGameTitle').innerText = selectedGame.title;
    document.getElementById('modalGamePublisher').innerText = selectedGame.publisher;
    document.getElementById('modalGameIcon').innerHTML = `<i class="fa-solid ${selectedGame.icon}"></i>`;

    // Render Account Fields
    const fieldsContainer = document.getElementById('accountInputFields');
    fieldsContainer.innerHTML = selectedGame.fields.map(f => `
        <div>
            <label class="block text-xs font-semibold text-gray-300 uppercase tracking-wider mb-1">${f.label}</label>
            <input type="${f.type}" id="input_${f.id}" placeholder="${f.placeholder}" class="w-full bg-brand-dark border border-brand-border rounded-xl px-4 py-2.5 text-white text-sm input-glow">
        </div>
    `).join('');

    // Render Nominals
    const nominalContainer = document.getElementById('nominalGrid');
    nominalContainer.innerHTML = selectedGame.nominals.map(n => `
        <div onclick="selectNominal(${n.id})" id="nominal_card_${n.id}" class="nominal-card bg-brand-dark border border-brand-border hover:border-cyan-500 rounded-xl p-3 cursor-pointer transition flex flex-col justify-between">
            <span class="text-xs font-bold text-white block">${n.name}</span>
            <span class="text-xs text-cyan-400 font-semibold mt-2">Rp ${n.price.toLocaleString('id-ID')}</span>
        </div>
    `).join('');

    // Render Payment Methods
    const paymentContainer = document.getElementById('paymentMethodsArea');
    const paymentMethods = [
        { id: 'qris', name: 'QRIS (All E-Wallet / Bank)', icon: 'fa-qrcode', color: 'text-cyan-400' },
        { id: 'dana', name: 'DANA / OVO / GoPay', icon: 'fa-wallet', color: 'text-purple-400' },
        { id: 'va', name: 'Virtual Account Bank', icon: 'fa-building-columns', color: 'text-blue-400' }
    ];
    paymentContainer.innerHTML = paymentMethods.map(p => `
        <div onclick="selectPayment('${p.id}', '${p.name}')" id="payment_card_${p.id}" class="payment-card bg-brand-dark border border-brand-border hover:border-cyan-500 rounded-xl p-3 cursor-pointer transition flex items-center justify-between">
            <div class="flex items-center gap-3">
                <i class="fa-solid ${p.icon} ${p.color} text-lg"></i>
                <span class="text-xs font-semibold text-gray-200">${p.name}</span>
            </div>
            <i class="fa-solid fa-chevron-right text-xs text-gray-500"></i>
        </div>
    `).join('');

    document.getElementById('topupModal').classList.remove('hidden');
}

function closeTopupModal() {
    document.getElementById('topupModal').classList.add('hidden');
}

function selectNominal(id) {
    selectedNominal = selectedGame.nominals.find(n => n.id === id);
    document.querySelectorAll('.nominal-card').forEach(c => c.classList.remove('border-cyan-500', 'bg-cyan-500/10'));
    
    const selectedCard = document.getElementById(`nominal_card_${id}`);
    if (selectedCard) selectedCard.classList.add('border-cyan-500', 'bg-cyan-500/10');
    
    updateTotalPrice();
}

function selectPayment(id, name) {
    selectedPayment = { id, name };
    document.querySelectorAll('.payment-card').forEach(c => c.classList.remove('border-cyan-500', 'bg-cyan-500/10'));
    
    const selectedCard = document.getElementById(`payment_card_${id}`);
    if (selectedCard) selectedCard.classList.add('border-cyan-500', 'bg-cyan-500/10');
}

function updateTotalPrice() {
    const priceDisplay = document.getElementById('displayTotalPrice');
    if (selectedNominal) {
        priceDisplay.innerText = `Rp ${selectedNominal.price.toLocaleString('id-ID')}`;
    } else {
        priceDisplay.innerText = 'Rp 0';
    }
}

// --- PURCHASE SUMMARY & INVOICE LOGIC ---
function processPurchaseSummary() {
    // Validate inputs
    let targetAccountText = '';
    for (let f of selectedGame.fields) {
        const val = document.getElementById(`input_${f.id}`)?.value.trim();
        if (!val) {
            showToast(`Harap isi field ${f.label} terlebih dahulu!`);
            return;
        }
        targetAccountText += `${val} `;
    }

    if (!selectedNominal) {
        showToast('Silakan pilih nominal top up!');
        return;
    }
    if (!selectedPayment) {
        showToast('Silakan pilih metode pembayaran!');
        return;
    }

    const phoneVal = document.getElementById('userPhone')?.value.trim();
    if (!phoneVal) {
        showToast('Masukkan nomor WhatsApp untuk bukti transaksi!');
        return;
    }

    // Generate Invoice ID
    const randomInvoice = 'RFM-' + Date.now().toString().slice(-8);

    // Fill Summary Modal
    document.getElementById('sumInvoiceId').innerText = randomInvoice;
    document.getElementById('sumGameTitle').innerText = selectedGame.title;
    document.getElementById('sumAccountTarget').innerText = targetAccountText.trim();
    document.getElementById('sumItemName').innerText = selectedNominal.name;
    document.getElementById('sumPaymentMethod').innerText = selectedPayment.name;
    document.getElementById('sumPhoneNumber').innerText = phoneVal;
    document.getElementById('sumTotalAmount').innerText = `Rp ${selectedNominal.price.toLocaleString('id-ID')}`;

    document.getElementById('summaryModal').classList.remove('hidden');
}

function closeSummaryModal() {
    document.getElementById('summaryModal').classList.add('hidden');
}

function confirmAndPay() {
    closeSummaryModal();
    closeTopupModal();
    showToast('Pesanan berhasil dibuat! Mengalihkan ke halaman pembayaran...');
}

// --- CHECK INVOICE STATUS ---
function checkInvoiceStatus() {
    const inputVal = document.getElementById('invoiceCheckInput')?.value.trim();
    const resultArea = document.getElementById('invoiceResultArea');

    if (!inputVal) {
        showToast('Masukkan No. Invoice terlebih dahulu!');
        return;
    }

    resultArea.classList.remove('hidden');
    resultArea.innerHTML = `
        <div class="bg-brand-dark border border-emerald-500/30 rounded-xl p-4 space-y-2">
            <div class="flex items-center justify-between">
                <span class="text-xs text-gray-400">Invoice: <strong class="text-white">${inputVal}</strong></span>
                <span class="px-2.5 py-1 bg-emerald-500/20 text-emerald-400 text-[11px] font-bold rounded-full">BERHASIL</span>
            </div>
            <p class="text-xs text-gray-300 pt-2 border-t border-brand-border">Transaksi telah sukses diproses. Item sudah masuk ke akun tujuan secara instan.</p>
        </div>
    `;
}

// --- TOAST NOTIFICATION ---
function showToast(msg) {
    const toast = document.getElementById('toast');
    const toastMsg = document.getElementById('toastMessage');
    if (!toast || !toastMsg) return;

    toastMsg.innerText = msg;
    toast.classList.remove('hidden');
    setTimeout(() => {
        toast.classList.add('hidden');
    }, 3000);
}
