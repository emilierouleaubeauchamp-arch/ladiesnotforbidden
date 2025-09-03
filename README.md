<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Golf Fashion Femme / Women's Golf Fashion</title>
<style>
  body { font-family: Arial, sans-serif; margin: 0; background: #f0f4f8; }
  header { background: #2a8fbd; color: white; padding: 1rem; display: flex; justify-content: space-between; align-items: center; }
  header h1 { margin: 0; font-size: 1.4rem; }
  nav { }
  select { font-size: 1rem; padding: 0.2rem 0.5rem; border-radius: 6px; border: none; }
  main { max-width: 960px; margin: 2rem auto; }
  .products { display: grid; grid-template-columns: repeat(auto-fill,minmax(260px,1fr)); gap: 1.2rem; }
  .product { background: white; border-radius: 10px; padding: 1rem; box-shadow: 0 3px 8px rgba(0,0,0,0.12); transition: transform 0.2s; }
  .product:hover { transform: translateY(-5px); }
  .product img { width: 100%; border-radius: 10px; }
  .product h2 { font-size: 1.1rem; margin: 0.5rem 0; color: #2a8fbd; }
  .product p { font-size: 0.9rem; color: #444; min-height: 48px; }
  .price { font-weight: bold; color: #205375; font-size: 1.1rem; margin-top: 0.5rem; }
  button { background: #2a8fbd; color: white; border: none; padding: 0.5rem 0.9rem; border-radius: 6px; font-size: 1rem; cursor: pointer; margin-top: 0.5rem; }
  button:hover { background: #1f6a94; }
  #cart { position: fixed; top: 10px; right: 10px; background: #2a8fbd; color: white; padding: 0.5rem 1rem; border-radius: 20px; cursor: pointer; font-weight: bold; z-index: 1100; }
  #cartCount { margin-left: 0.4rem; }
  #cartDetails { display: none; position: fixed; top: 50px; right: 10px; background: white; border: 1px solid #ddd; padding: 1rem; width: 320px; max-height: 450px; overflow-y: auto; box-shadow: 0 2px 8px rgba(0,0,0,0.2); border-radius: 10px; z-index: 1200; }
  #closeCart { float: right; cursor: pointer; font-weight: bold; color: #555; }
  .cartItem { border-bottom: 1px solid #eee; margin-bottom: 0.8rem; padding-bottom: 0.8rem; }
  .cartTotal { font-weight: bold; margin-top: 1rem; font-size: 1.1rem; color: #205375; }
</style>
</head>
<body>
<header>
  <h1 id="title">Golf Fashion Femme</h1>
  <nav>
    <label for="langSelect" style="color:white;">Langue / Language:</label>
    <select id="langSelect" aria-label="Choisir langue / Choose language">
      <option value="fr" selected>Français</option>
      <option value="en">English</option>
    </select>
  </nav>
</header>

<button id="cart">Panier (0)</button>
<div id="cartDetails">
  <span id="closeCart">✖</span>
  <h3 id="cartTitle">Votre Panier</h3>
  <div id="cartItems"></div>
  <div class="cartTotal" id="cartTotalText">Total : 0 €</div>
</div>

<main>
  <section class="products" id="productsContainer"></section>
</main>

<script>
const langSelect = document.getElementById('langSelect');
const titleEl = document.getElementById('title');
const cartBtn = document.getElementById('cart');
const cartDetails = document.getElementById('cartDetails');
const closeCartBtn = document.getElementById('closeCart');
const cartItemsDiv = document.getElementById('cartItems');
const cartTotalText = document.getElementById('cartTotalText');
const cartTitle = document.getElementById('cartTitle');

const productsData = {
  fr: [
    {id:1, name:"Polo Femme Golf Élégant", desc:"Polo respirant et stylé pour vos parties de golf.", price:59.99, img:"https://via.placeholder.com/260x180?text=Polo+Golf", url:"#"},
    {id:2, name:"Casquette Golf Femme", desc:"Casquette légère et ajustable, parfaite pour le golf.", price:29.99, img:"https://via.placeholder.com/260x180?text=Casquette+Golf", url:"#"},
    {id:3, name:"Jupe de Golf Sportive", desc:"Jupe confortable et féminine, idéale pour jouer.", price:49.99, img:"https://via.placeholder.com/260x180?text=Jupe+Golf", url:"#"},
    {id:4, name:"Gants de Golf Roses", desc:"Gants pour une meilleure prise en main sur le club.", price:19.99, img:"https://via.placeholder.com/260x180?text=Gants+Golf", url:"#"},
    {id:5, name:"Chaussures Golf Femme", desc:"Chaussures antidérapantes pour parcours de golf.", price:99.99, img:"https://via.placeholder.com/260x180?text=Chaussures+Golf", url:"#"},
    {id:6, name:"Balles de Golf", desc:"Série de balles de golf adaptées aux femmes.", price:24.99, img:"https://via.placeholder.com/260x180?text=Balles+Golf", url:"#"},
    {id:7, name:"Tee de Golf", desc:"Tee résistant pour améliorer vos drives.", price:9.99, img:"https://via.placeholder.com/260x180?text=Tee+Golf", url:"#"}
  ],
  en: [
    {id:1, name:"Stylish Women's Golf Polo", desc:"Breathable and stylish polo for your golf rounds.", price:59.99, img:"https://via.placeholder.com/260x180?text=Golf+Polo", url:"#"},
    {id:2, name:"Women's Golf Cap", desc:"Lightweight and adjustable cap, perfect for golf.", price:29.99, img:"https://via.placeholder.com/260x180?text=Golf+Cap", url:"#"},
    {id:3, name:"Sporty Golf Skirt", desc:"Comfortable and feminine skirt, ideal for playing.", price:49.99, img:"https://via.placeholder.com/260x180?text=Golf+Skirt", url:"#"},
    {id:4, name:"Pink Golf Gloves", desc:"Gloves for better grip on the club.", price:19.99, img:"https://via.placeholder.com/260x180?text=Golf+Gloves", url:"#"},
    {id:5, name:"Women's Golf Shoes", desc:"Slip-resistant shoes for golf courses.", price:99.99, img:"https://via.placeholder.com/260x180?text=Golf+Shoes", url:"#"},
    {id:6, name:"Golf Balls", desc:"Set of golf balls suited for women.", price:24.99, img:"https://via.placeholder.com/260x180?text=Golf+Balls", url:"#"},
    {id:7, name:"Golf Tees", desc:"Durable tees to improve your drives.", price:9.99, img:"https://via.placeholder.com/260x180?text=Golf+Tees", url:"#"}
  ]
};

let currentLang = 'fr';
let cart = [];

function renderProducts(){
  const container = document.getElementById('productsContainer');
  container.innerHTML = '';
  productsData[currentLang].forEach(prod => {
    const div = document.createElement('div');
    div.className = 'product';
    div.innerHTML = `
      <img src="${prod.img}" alt="${prod.name}" />
      <h2>${prod.name}</h2>
      <p>${prod.desc}</p>
      <div class="price">${prod.price.toFixed(2)} ${currentLang==='fr' ? '€' : '$'}</div>
      <button onclick="addToCart(${prod.id})">${currentLang==='fr' ? 'Ajouter au panier' : 'Add to cart'}</button>
    `;
    container.appendChild(div);
  });
  updateCartUI();
}

function addToCart(id){
  const product = productsData[currentLang].find(p => p.id === id);
  const found = cart.find(item => item.id === id);
  if(found){
    found.qty++;
  } else {
    cart.push({...product, qty:1});
  }
  updateCartUI();
}

function updateCartUI(){
  const totalQty = cart.reduce((acc,item) => acc + item.qty, 0);
  cartBtn.textContent = currentLang==='fr' ? `Panier (${totalQty})` : `Cart (${totalQty})`;
  cartItemsDiv.innerHTML = '';
  let totalPrice = 0;
  cart.forEach(item => {
    totalPrice += item.price * item.qty;
    const div = document.createElement('div');
    div.className = 'cartItem';
    div.innerHTML = `
      <strong>${item.name}</strong> x${item.qty}<br />
      ${(item.price.toFixed(2))} ${currentLang==='fr' ? '€' : '$'} x ${item.qty} = ${(item.price * item.qty).toFixed(2)} ${currentLang==='fr' ? '€' : '$'}
    `;
    cartItemsDiv.appendChild(div);
  });
  cartTotalText.textContent = (currentLang==='fr' ? 'Total : ' : 'Total: ') + totalPrice.toFixed(2) + (currentLang==='fr' ? ' €' : ' $');
  cartTitle.textContent = currentLang==='fr' ? 'Votre Panier' : 'Your Cart';
}

cartBtn.addEventListener('click', () => {
  cartDetails.style.display = cartDetails.style.display === 'block' ? 'none' : 'block';
});

closeCartBtn.addEventListener('click', () => {
  cartDetails.style.display = 'none';
});

langSelect.addEventListener('change', e => {
  currentLang = e.target.value;
  titleEl.textContent = currentLang === 'fr' ? 'Golf Fashion Femme' : "Women's Golf Fashion";
  renderProducts();
});

renderProducts();
</script>
</body>
</html>
