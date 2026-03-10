<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Hemankaro Grocery</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:'Poppins',sans-serif;
}

body{
background:#f6f7f9;
color:#333;
}

header{
background:#1e8e3e;
color:white;
padding:20px;
text-align:center;
}

header h1{
font-size:28px;
}

header p{
font-size:14px;
opacity:0.9;
}

.hero{
background:url("https://img.freepik.com/free-photo/happy-beautiful-couple-posing-with-shopping-bags-violet_18348761.htm") center/cover;
height:200px;
display:flex;
align-items:center;
justify-content:center;
color:white;
font-size:22px;
font-weight:600;
text-shadow:0 3px 8px rgba(0,0,0,0.5);
}

.filters{
display:flex;
gap:10px;
padding:15px;
overflow:auto;
}

.filters button{
border:none;
padding:8px 14px;
background:#eaeaea;
border-radius:20px;
cursor:pointer;
}

.filters button.active{
background:#1e8e3e;
color:white;
}

.products{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(160px,1fr));
gap:15px;
padding:15px;
}

.card{
background:white;
border-radius:10px;
padding:10px;
box-shadow:0 2px 6px rgba(0,0,0,0.1);
}

.card img{
width:100%;
height:120px;
object-fit:cover;
border-radius:6px;
}

.card h4{
font-size:14px;
margin:6px 0;
}

.price{
color:#1e8e3e;
font-weight:600;
}

.card button{
width:100%;
border:none;
background:#1e8e3e;
color:white;
padding:8px;
margin-top:5px;
border-radius:5px;
cursor:pointer;
}

.cart-btn{
position:fixed;
bottom:20px;
right:20px;
background:#ff9800;
color:white;
width:55px;
height:55px;
border-radius:50%;
display:flex;
align-items:center;
justify-content:center;
font-size:22px;
cursor:pointer;
box-shadow:0 4px 10px rgba(0,0,0,0.3);
}

.cart-count{
position:absolute;
top:-5px;
right:-5px;
background:red;
color:white;
font-size:12px;
padding:3px 6px;
border-radius:50%;
}

.cart-modal{
position:fixed;
top:0;
left:0;
width:100%;
height:100%;
background:rgba(0,0,0,0.6);
display:none;
justify-content:center;
align-items:center;
}

.cart-box{
background:white;
width:95%;
max-width:420px;
max-height:90%;
overflow:auto;
padding:15px;
border-radius:10px;
}

.cart-item{
display:flex;
justify-content:space-between;
margin-bottom:10px;
}

.qty{
display:flex;
gap:6px;
align-items:center;
}

.qty button{
padding:2px 8px;
}

.total{
font-weight:600;
margin:10px 0;
}

.checkout{
background:#1e8e3e;
color:white;
border:none;
padding:10px;
width:100%;
cursor:pointer;
}

form{
margin-top:10px;
}

input,textarea{
width:100%;
padding:8px;
margin-bottom:8px;
}

.whatsapp{
position:fixed;
bottom:20px;
left:20px;
background:#25D366;
color:white;
padding:12px 16px;
border-radius:30px;
text-decoration:none;
}

</style>
</head>

<body>

<header>
<h1>Hemankaro</h1>
<p>Fresh groceries delivered to your door</p>
<p>Narwana, Jind, HR | 📞 7814056100</p>
</header>

<div class="hero">
Fresh Fruits & Groceries
</div>

<div class="filters">
<button class="active" onclick="filter('all')">All</button>
<button onclick="filter('Fruits')">Fruits</button>
<button onclick="filter('Groceries')">Groceries</button>
<button onclick="filter('Dairy')">Dairy</button>
<button onclick="filter('Snacks')">Snacks</button>
<button onclick="filter('Beverages')">Beverages</button>
</div>

<div class="products" id="products"></div>

<div class="cart-btn" onclick="toggleCart()">
🛒
<div class="cart-count" id="count">0</div>
</div>

<div class="cart-modal" id="cartModal">

<div class="cart-box">

<h3>Your Cart</h3>

<div id="cartItems"></div>

<div class="total" id="grandTotal"></div>

<button class="checkout" onclick="showForm()">Place Order</button>

<form id="orderForm" action="https://formspree.io/f/mreypqbr" method="POST" style="display:none">

<input required name="name" placeholder="Customer Name">

<input required name="phone" placeholder="Phone Number">

<textarea required name="address" placeholder="Delivery Address"></textarea>

<textarea name="note" placeholder="Special instructions"></textarea>

<input type="hidden" name="order" id="orderSummary">

<button class="checkout">Submit Order</button>

</form>

</div>

</div>

<a class="whatsapp" href="https://wa.me/7814056100">WhatsApp</a>

<script>

const products=[
{name:"Mangoes 1kg",price:150,cat:"Fruits",img:"https://i.ibb.co/9HBY0Rt5/Screenshot-2026-03-08-233017.png",desc:"Fresh mangoes"},
{name:"Orange 1kg",price:100,cat:"Fruits",img:"https://i.ibb.co/qMmm5GHG/Screenshot-2026-03-08-233123.png",desc:"Fresh oranges"},
{name:"Grapes 1kg",price:200,cat:"Fruits",img:"https://i.ibb.co/hxdcs3p6/pexels-luiz-m-santos-250917-760281.jpg",desc:"Sweet grapes"},
{name:"Bananas 1 dozen",price:60,cat:"Fruits",img:"https://i.ibb.co/hxdcs3p6/pexels-luiz-m-santos-250917-760281.jpg",desc:"Healthy bananas"},
{name:"Aashirvaad Atta 5kg",price:280,cat:"Groceries",img:"https://i.ibb.co/hxdcs3p6/pexels-luiz-m-santos-250917-760281.jpg",desc:"Whole wheat flour"},
{name:"Amul Butter",price:280,cat:"Dairy",img:"https://i.ibb.co/hxdcs3p6/pexels-luiz-m-santos-250917-760281.jpg",desc:"Fresh butter"},
{name:"Milk 1L",price:60,cat:"Dairy",img:"https://i.ibb.co/hxdcs3p6/pexels-luiz-m-santos-250917-760281.jpg",desc:"Fresh dairy milk"},
{name:"Biscuits Pack",price:40,cat:"Snacks",img:"https://i.ibb.co/hxdcs3p6/pexels-luiz-m-santos-250917-760281.jpg",desc:"Crunchy biscuits"},
{name:"Chips Pack",price:30,cat:"Snacks",img:"https://i.ibb.co/hxdcs3p6/pexels-luiz-m-santos-250917-760281.jpg",desc:"Potato chips"},
{name:"Coca Cola",price:40,cat:"Beverages",img:"https://i.ibb.co/hxdcs3p6/pexels-luiz-m-santos-250917-760281.jpg",desc:"Cold drink"}
]

let cart=JSON.parse(localStorage.getItem("cart"))||[]

function renderProducts(list){

const container=document.getElementById("products")

container.innerHTML=""

list.forEach((p,i)=>{

container.innerHTML+=`
<div class="card">

<img src="${p.img}" loading="lazy" alt="${p.name}">

<h4>${p.name}</h4>

<p>${p.desc}</p>

<div class="price">₹${p.price}</div>

<button onclick="addCart(${i})">Add to Cart</button>

</div>
`

})

}

renderProducts(products)

function filter(cat){

if(cat==="all") renderProducts(products)
else renderProducts(products.filter(p=>p.cat===cat))

}

function addCart(i){

let item=cart.find(x=>x.name===products[i].name)

if(item) item.qty++

else cart.push({...products[i],qty:1})

saveCart()

}

function saveCart(){

localStorage.setItem("cart",JSON.stringify(cart))

updateCart()

}

function updateCart(){

document.getElementById("count").innerText=cart.reduce((a,b)=>a+b.qty,0)

let html=""

let total=0

cart.forEach((item,i)=>{

let sub=item.price*item.qty

total+=sub

html+=`
<div class="cart-item">

<div>
${item.name}
<br>₹${item.price}
</div>

<div class="qty">

<button onclick="changeQty(${i},-1)">-</button>

${item.qty}

<button onclick="changeQty(${i},1)">+</button>

<button onclick="removeItem(${i})">x</button>

</div>

</div>
`

})

document.getElementById("cartItems").innerHTML=html

document.getElementById("grandTotal").innerText="Total: ₹"+total

document.getElementById("orderSummary").value=
cart.map(i=>`${i.name} x${i.qty} = ₹${i.price*i.qty}`).join(", ")+" | Total ₹"+total

}

function changeQty(i,v){

cart[i].qty+=v

if(cart[i].qty<=0) cart.splice(i,1)

saveCart()

}

function removeItem(i){

cart.splice(i,1)

saveCart()

}

function toggleCart(){

document.getElementById("cartModal").style.display="flex"

}

function showForm(){

document.getElementById("orderForm").style.display="block"

}

updateCart()

</script>

</body>
</html>
