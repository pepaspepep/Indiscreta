# Indiscreta
Venta de perfumes y variedad de moda
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Indiscreta • Perfumes Importados</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: Arial, sans-serif; background:#f5f5f5; color:#333; }
    header {
      background:#fff;
      padding:10px 20px;
      display:flex;
      align-items:center;
      justify-content: space-between;
      flex-wrap: wrap;
      box-shadow:0 2px 4px rgba(0,0,0,0.1);
      position: sticky;
      top: 0;
      z-index: 1000;
    }
    header .logo { height:50px; }
    header input[type=search] {
      flex: 1;
      min-width: 200px;
      margin-left:20px;
      padding:8px 12px;
      border-radius:20px;
      border:1px solid #ccc;
    }
    .container {
      max-width:1200px;
      margin:20px auto;
      display:flex;
      flex-wrap: wrap;
      gap:20px;
      padding: 0 10px;
    }
    .products {
      flex: 1;
      display:grid;
      grid-template-columns:repeat(auto-fill,minmax(220px,1fr));
      gap:20px;
      min-width: 250px;
    }
    .card {
      background:#fff;
      border-radius:8px;
      box-shadow:0 2px 5px rgba(0,0,0,0.1);
      padding:15px;
      display:flex;
      flex-direction:column;
      transition: transform 0.2s;
    }
    .card:hover { transform: translateY(-5px); }
    .card img { width:100%; height:200px; object-fit:contain; margin-bottom:10px; border-radius:4px; }
    .card h3 { margin-bottom:6px; font-size:18px; }
    .card p { margin-bottom:8px; font-size:14px; color:#555; flex:1; }
    .price { font-size:16px; font-weight:bold; color:#d93025; margin-bottom:10px; }
    .card button { padding:8px; border:none; border-radius:4px; cursor:pointer; color:#fff; background:#d93025; transition: background 0.2s; }
    .card button:hover { background:#b1271b; }
    aside.cart {
      background:#fff;
      padding:15px;
      border-radius:8px;
      box-shadow:0 2px 5px rgba(0,0,0,0.1);
      flex-basis: 300px;
      min-width: 250px;
      max-height: 90vh;
      display: flex;
      flex-direction: column;
    }
    aside.cart h2 { margin-bottom:10px; }
    .cart-list { overflow:auto; flex:1; margin-bottom:10px; }
    .cart-item { display:flex; justify-content: space-between; margin-bottom:8px; font-size:14px; }
    .cart-item button { background:none; border:none; color:#d93025; cursor:pointer; font-weight:bold; margin-left:5px; }
    .total { font-weight:bold; margin-top:10px; font-size:16px; }
    .checkout button { width:100%; padding:10px; border:none; border-radius:4px; font-size:16px; margin-top:5px; cursor:pointer; transition: background 0.2s; }
    .clear { background:#ccc; color:#333; }
    .clear:hover { background:#999; }
    .buy { background:#4285f4; color:#fff; }
    .buy:hover { background:#3367d6; }
    footer { text-align:center; margin:20px 0; color:#777; font-size:14px; padding:10px; }
    @media (max-width: 768px) {
      header { flex-direction: column; align-items: flex-start; }
      header input[type=search] { margin:10px 0 0 0; width: 100%; }
      .container { flex-direction: column; }
      aside.cart { width: 100%; max-height: none; }
    }
  </style>
</head>
<body>

<header>
  <img src="https://via.placeholder.com/150x50?text=Logo" alt="Indiscreta Logo" class="logo" />
  <input type="search" placeholder="Buscar perfumes..." id="search" />
</header>

<div class="container">
  <div class="products" id="products"></div>

  <aside class="cart">
    <h2>Carrito</h2>
    <div class="cart-list" id="cart-list">No hay productos aún.</div>
    <div class="total" id="total">Total: $0</div>
    <button class="clear" id="clear-cart">Vaciar carrito</button>
    <button class="buy" id="checkout">Finalizar compra</button>
    <div id="checkout-result" style="margin-top:10px; font-size:14px;"></div>
  </aside>
</div>

<footer>Contacto: WhatsApp +54 9 2245517452 o 2245503966 · Envíos a todo el país</footer>

<script>
  const PRODUCTS = [
    {id:1, name:'Perfume Lattafa Khamrah', price:65000, img:'https://via.placeholder.com/200x200?text=Lattafa+Khamrah', desc:'Oriental-Spicy con canela, vainilla, praliné y sándalo.'},
    {id:2, name:'Fakhar Pride Negro', price:60000, img:'https://via.placeholder.com/200x200?text=Fakhar+Pride', desc:'Fougère-madera, con manzana, lavanda, tonka, cedro.'},
    {id:3, name:'Asad Zanzíbar', price:50000, img:'https://via.placeholder.com/200x200?text=Asad+Zanzibar', desc:'Especiado-fresco con pimienta, coco, vainilla y incienso.'},
    {id:4, name:'Mandarín Sky', price:75000, img:'https://via.placeholder.com/200x200?text=Mandarin+Sky', desc:'Cítrico-musk con mandarina, bergamota y maderas.'},
    {id:5, name:'Teriaq', price:60000, img:'https://via.placeholder.com/200x200?text=Teriaq', desc:'Dulce-especiado, con caramelo, rosa, miel, vainilla, cuero.'},
    {id:6, name:'Dragón Sehr', price:60000, img:'https://via.placeholder.com/200x200?text=Dragon+Sehr', desc:'Flor-oriental con almendra, jazmín, vainilla y ámbar.'},
    {id:7, name:'Maison Rosa', price:40000, img:'https://via.placeholder.com/200x200?text=Maison+Rosa', desc:'Rosa floral elegante y suave.'},
    {id:8, name:'Mayar Rosa y Celeste', price:50000, img:'https://via.placeholder.com/200x200?text=Mayar', desc:'Floral-frutal: lichi, rosa, vainilla y almizcle.'},
    {id:9, name:'Haramain Amber Oud', price:100000, img:'https://via.placeholder.com/200x200?text=Amber+Oud', desc:'Amaderado-oud con ámbar profundo.'}
  ];

  const fmt = n => '$' + new Intl.NumberFormat('de-DE').format(n);
  let cart = {};

  const productsEl = document.getElementById('products');
  PRODUCTS.forEach(p => {
    const div = document.createElement('div');
    div.className = 'card';
    div.innerHTML = `
      <img src="${p.img}" alt="${p.name}">
      <h3>${p.name}</h3>
      <p>${p.desc}</p>
      <div class="price">${fmt(p.price)}</div>
      <button data-id="${p.id}">Agregar al carrito</button>
    `;
    productsEl.appendChild(div);
    div.querySelector('button').onclick = () => {
      cart[p.id] = (cart[p.id] || { ...p, qty: 0 });
      cart[p.id].qty++;
      renderCart();
    };
  });

  function renderCart() {
    const list = document.getElementById('cart-list');
    list.innerHTML = '';
    let total = 0;
    for (let id in cart) {
      const item = cart[id];
      total += item.qty * item.price;
      const row = document.createElement('div');
      row.className = 'cart-item';
      row.innerHTML = `
        <span>${item.qty} × ${item.name}</span>
        <span>${fmt(item.qty * item.price)} <button onclick="removeItem(${id})">X</button></span>
      `;
      list.appendChild(row);
    }
    document.getElementById('total').textContent = 'Total: ' + fmt(total);
  }

  function removeItem(id) {
    delete cart[id];
    renderCart();
  }

  document.getElementById('clear-cart').onclick = () => {
    cart = {};
    renderCart();
    document
