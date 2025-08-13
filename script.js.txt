let cart = [];
let language = 'ar';

const translations = {
    ar: {
        siteTitle: "متجر الورود 🌹",
        navHome: "الرئيسية",
        navCart: "السلة 🛒",
        navContact: "اتصل بنا",
        productsTitle: "منتجاتنا",
        cartTitle: "سلتك",
        thProduct: "المنتج",
        thQuantity: "الكمية",
        thPrice: "السعر",
        thRemove: "إزالة",
        totalPrice: "الإجمالي",
        checkout: "شراء الآن",
        closeCart: "اغلق السلة",
        addedToCart: "تمت الإضافة إلى السلة! 🛒",
        checkoutAlert: "شكراً لطلبك!\nسيتم التواصل معك لتأكيد الطلب."
    },
    en: {
        siteTitle: "Flower Shop 🌹",
        navHome: "Home",
        navCart: "Cart 🛒",
        navContact: "Contact Us",
        productsTitle: "Our Products",
        cartTitle: "Your Cart",
        thProduct: "Product",
        thQuantity: "Quantity",
        thPrice: "Price",
        thRemove: "Remove",
        totalPrice: "Total",
        checkout: "Checkout",
        closeCart: "Close Cart",
        addedToCart: "Added to cart! 🛒",
        checkoutAlert: "Thank you for your order!\nWe will contact you to confirm it."
    }
};

function setLanguage(lang){
    language = lang;
    document.getElementById('site-title').textContent = translations[lang].siteTitle;
    document.getElementById('nav-home').textContent = translations[lang].navHome;
    document.getElementById('nav-cart').innerHTML = `${translations[lang].navCart} (<span id="cart-count">${cartCount()}</span>)`;
    document.getElementById('nav-contact').textContent = translations[lang].navContact;
    document.getElementById('products-title').textContent = translations[lang].productsTitle;
    document.getElementById('cart-title').textContent = translations[lang].cartTitle;
    document.getElementById('th-product').textContent = translations[lang].thProduct;
    document.getElementById('th-quantity').textContent = translations[lang].thQuantity;
    document.getElementById('th-price').textContent = translations[lang].thPrice;
    document.getElementById('th-remove').textContent = translations[lang].thRemove;
    document.getElementById('checkout-btn').textContent = translations[lang].checkout;
    document.getElementById('close-cart-btn').textContent = translations[lang].closeCart;

    document.querySelectorAll('.product').forEach(prod => {
        prod.querySelector('.product-name').textContent = language === 'ar' ? prod.dataset.arName : prod.dataset.enName;
        prod.querySelector('.product-price').textContent = language === 'ar' ? `سعر: ${prod.querySelector('.add-cart-btn').dataset.price} ريال` : `Price: ${prod.querySelector('.add-cart-btn').dataset.price} SAR`;
    });

    renderCart();
}

function cartCount(){ return cart.reduce((sum,item)=>sum+item.quantity,0); }

function addToCart(productName, price){
    let item = cart.find(p=>p.name===productName);
    if(item) item.quantity++;
    else cart.push({name:productName,price:parseInt(price),quantity:1});
    alert(translations[language].addedToCart);
    renderCart();
    updateCartCount();
}

function updateCartCount(){
    document.getElementById('cart-count').textContent = cartCount();
    document.getElementById('nav-cart').innerHTML = `${translations[language].navCart} (<span id="cart-count">${cartCount()}</span>)`;
}

function showCart(){ document.getElementById('cart').style.display='block'; renderCart(); }
function closeCart(){ document.getElementById('cart').style.display='none'; }

function renderCart(){
    let tbody = document.querySelector('#cart-table tbody');
    tbody.innerHTML='';
    let totalPrice=0;
    cart.forEach((item,index)=>{
        totalPrice+=item.price*item.quantity;
        let row=document.createElement('tr');
        row.innerHTML=`
            <td>${item.name}</td>
            <td><input type="number" min="1" value="${item.quantity}" onchange="updateQuantity(${index}, this.value)"></td>
            <td>${item.price*item.quantity} ${language==='ar'?'ريال':'SAR'}</td>
            <td><button onclick="removeItem(${index})">${translations[language].thRemove}</button></td>
        `;
        tbody.appendChild(row);
    });
    document.getElementById('total-price').textContent=`${translations[language].totalPrice}: ${totalPrice} ${language==='ar'?'ريال':'SAR'}`;
}

function updateQuantity(index,value){ cart[index].quantity=parseInt(value); renderCart(); updateCartCount(); }
function removeItem(index){ cart.splice(index,1); renderCart(); updateCartCount(); }

function checkout(){
    if(cart.length===0){ alert(language==='ar'?"سلتك فارغة!":"Your cart is empty!"); return; }
    alert(translations[language].checkoutAlert);
    cart=[];
    renderCart();
    updateCartCount();
    closeCart();
}

setLanguage('ar');
