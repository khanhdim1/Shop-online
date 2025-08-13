// Modal Auth
const authModal = document.getElementById('authModal');
const loginForm = document.getElementById('loginForm');
const registerForm = document.getElementById('registerForm');

function showLogin() { loginForm.style.display='block'; registerForm.style.display='none'; authModal.style.display='flex'; }
function showRegister() { loginForm.style.display='none'; registerForm.style.display='block'; authModal.style.display='flex'; }
function closeModal() { authModal.style.display='none'; }

// LocalStorage Auth
function register() {
    const user = document.getElementById('regUser').value;
    const pass = document.getElementById('regPass').value;
    if(user && pass){ localStorage.setItem(user, pass); alert('Đăng ký thành công!'); closeModal(); }
    else alert('Vui lòng điền đầy đủ thông tin.');
}
function login() {
    const user = document.getElementById('loginUser').value;
    const pass = document.getElementById('loginPass').value;
    const storedPass = localStorage.getItem(user);
    if(storedPass && storedPass===pass){ alert('Đăng nhập thành công!'); closeModal(); }
    else alert('Tên đăng nhập hoặc mật khẩu sai.');
}

// Slideshow
let slideIndex = 0;
const slides = document.querySelectorAll('.slide');
function showSlides() {
    slides.forEach(s=>s.classList.remove('active'));
    slideIndex = (slideIndex+1)%slides.length;
    slides[slideIndex].classList.add('active');
    setTimeout(showSlides, 3000);
}
showSlides();

// Filter products
function filterCategory(cat){
    const products = document.querySelectorAll('.product');
    products.forEach(p=>{
        if(cat==='all'||p.dataset.category===cat)p.style.display='block';
        else p.style.display='none';
    });
}

// Cart
let cart = [];
function addToCart(button){
    const productDiv = button.parentElement;
    const name = productDiv.querySelector('h3').innerText;
    const price = parseInt(productDiv.querySelector('.price').innerText.replace('đ','').replace('.',''));
    cart.push({name, price});
    updateCart();
}

function updateCart(){
    const cartItems = document.getElementById('cart-items');
    const totalEl = document.getElementById('total');
    const countEl = document.getElementById('cart-count');
    cartItems.innerHTML = '';
    let total = 0;
    cart.forEach((item,index)=>{
        const li = document.createElement('li');
        li.innerHTML = `${item.name} - ${item.price}đ <button onclick="removeItem(${index})">X</button>`;
        cartItems.appendChild(li);
        total += item.price;
    });
    totalEl.innerText = total.toLocaleString();
    countEl.innerText = cart.length;
}

function removeItem(index){
    cart.splice(index,1);
    updateCart();
}

function toggleCart(){
    const modal = document.getElementById('cart');
    modal.style.display = (modal.style.display==='flex')?'none':'flex';
}

function checkout(){
    if(cart.length===0){ alert('Giỏ hàng trống'); return; }
    alert(`Thanh toán thành công! Tổng: ${cart.reduce((a,b)=>a+b.price,0).toLocaleString()}đ`);
    cart=[]; updateCart(); toggleCart();
}
