
<script setup>
import { ref } from 'vue';

// ESTADOS
const cargando = ref(true);
const categoria = ref('helados');
const carrito = ref([]);
const total = ref(0);
const verConfirmacion = ref(false);

// DATOS (30 PRODUCTOS)
const lista = ref([
  // HELADOS (10)
  { id: 1, n: 'Copa Amalia Suprema', p: 24500, c: 'helados', d: 'Tres bolas premium con salsa artesanal.', img: 'https://images.unsplash.com/photo-1563805042-7684c019e1cb?w=400' },
  { id: 2, n: 'Bola Vainilla Bourbon', p: 11000, c: 'helados', d: 'Vainilla natural de madagascar.', img: 'https://images.unsplash.com/photo-1570197788417-0e82375c9371?w=400' },
  { id: 3, n: 'Sorbet de Mango-Biche', p: 12000, c: 'helados', d: 'Refrescante con un toque de sal y limón.', img: 'https://images.unsplash.com/photo-1534706936160-d5ee67737249?w=400' },
  { id: 4, n: 'Gelato de Avellana', p: 16000, c: 'helados', d: 'Textura cremosa con avellanas tostadas.', img: 'https://images.unsplash.com/photo-1580915411954-282cb1b0d780?w=400' },
  { id: 5, n: 'Malteada de Caramelo', p: 18500, c: 'helados', d: 'Batido denso con fudge de caramelo.', img: 'https://www.cocinadelirante.com/800x600/filters:format(webp):quality(75)/sites/default/files/images/2024/10/malteada-caramelo.jpg' },
  { id: 6, n: 'Sundae de Chocolate', p: 15500, c: 'helados', d: 'Con brownie troceado y sirope caliente.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR7Pr1nHFvR_hAwbfLYl0o6RH545Lb_K7hsgg&s' },
  { id: 7, n: 'Cono Especial Kids', p: 8500, c: 'helados', d: 'Sabor chicle con chispas de colores.', img: 'https://www.diyoncrepes.com/wp-content/uploads/2020/06/Heladeria2.jpg' },
  { id: 8, n: 'Nieve de Maracuyá', p: 10500, c: 'helados', d: '100% pulpa de fruta natural.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRX29vxO8xonSRSfWPGHg491oylI5EnkX9dGg&s' },
  { id: 9, n: 'Paleta de Pistacho', p: 9500, c: 'helados', d: 'Rellena de crema de pistacho.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRtH5BKJiNLyFI0PSHi0TKYMjCJfkUBtd-WZw&s' },
  { id: 10, n: 'Banana Split Amalia', p: 27000, c: 'helados', d: 'El clásico con un toque gourmet.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRWVQhMDPCn3LA-3_APh7nRkxenWf1Xt4qUmg&s' },

  // CAFETERÍA (10)
  { id: 11, n: 'Flat White', p: 9800, c: 'cafes', d: 'Doble espresso con leche micro-cremosa.', img: 'https://www.lavazzausa.com/es/recipes-and-coffee-hacks/como-hacer-cafe-flat-white/_jcr_content/root/cust/customcontainer/image.coreimg.jpeg/1747815664322/d-m-how-to-slot-1-large%402.jpeg' },
  { id: 12, n: 'Latte de Vainilla', p: 11500, c: 'cafes', d: 'Suave y dulce para cualquier hora.', img: 'https://www.goodnes.com/sites/g/files/jgfbjl321/files/srh_recipes/5e7b1b6d1c09850b95ae3113f21f8606.jpg' },
  { id: 13, n: 'Cappuccino Tradicional', p: 9000, c: 'cafes', d: 'Equilibrio perfecto entre café y espuma.', img: 'https://images.unsplash.com/photo-1534778101976-62847782c213?w=400' },
  { id: 14, n: 'Espresso Sencillo', p: 5500, c: 'cafes', d: 'Energía pura en una taza pequeña.', img: 'https://images.unsplash.com/photo-1510591509098-f4fdc6d0ff04?w=400' },
  { id: 15, n: 'Mocaccino Dark', p: 12500, c: 'cafes', d: 'Café con chocolate amargo al 70%.', img: 'https://images.unsplash.com/photo-1610889556528-9a770e32642f?w=400' },
  { id: 16, n: 'Affogato Espresso', p: 14000, c: 'cafes', d: 'El puente entre helado y café.', img: 'https://www.thespruceeats.com/thmb/5PcCBEaUd1U1eFxfcKKPLVnZzfA=/1500x0/filters:no_upscale():max_bytes(150000):strip_icc()/affogato-4776668-hero-08-40d7a68d12ba46f48eaea3c43aba715c.jpg' },
  { id: 17, n: 'Cold Brew Citrus', p: 11000, c: 'cafes', d: 'Café frío con rodajas de naranja.', img: 'https://images.unsplash.com/photo-1461023058943-07fcbe16d735?w=400' },
  { id: 18, n: 'Americano de Origen', p: 6000, c: 'cafes', d: 'Café suave con notas frutales.', img: 'https://images.unsplash.com/photo-1495474472287-4d71bcdd2085?w=400' },
  { id: 19, n: 'Frappé Cookies', p: 16500, c: 'cafes', d: 'Granizado de café con galleta oreo.', img: 'https://images.unsplash.com/photo-1572490122747-3968b75cc699?w=400' },
  { id: 20, n: 'Té Chai Latte', p: 12000, c: 'cafes', d: 'Mezcla de especias y leche caliente.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQxTaVINXT7374sz6cR1vGNJ-FjcLJ82uH-mg&s' },

  // POSTRES (10)
  { id: 21, n: 'Tiramisú Casero', p: 15000, c: 'postres', d: 'Bizcocho empapado en café y mascarpone.', img: 'https://images.unsplash.com/photo-1571877227200-a0d98ea607e9?w=400' },
  { id: 22, n: 'Cheesecake Frutos', p: 14500, c: 'postres', d: 'Tarta de queso horneada con mermelada.', img: 'https://images.unsplash.com/photo-1533134242443-d4fd215305ad?w=400' },
  { id: 23, n: 'Waffle con Nutella', p: 17000, c: 'postres', d: 'Recién hecho, crujiente y dulce.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRCnI6d7R7f0aTkAbfPFVTbhXdFcatWVBvWNA&s' },
  { id: 24, n: 'Brownie de la Casa', p: 8000, c: 'postres', d: 'Chocolate intenso con nueces.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRIzmbySkOEQSrSvM9BKtzXFoMwyUtxoDA0tg&s' },
  { id: 25, n: 'Pie de Limón', p: 11000, c: 'postres', d: 'Equilibrio cítrico con merengue suizo.', img: 'https://images.unsplash.com/photo-1551024709-8f23befc6f87?w=400' },
  { id: 26, n: 'Torta Red Velvet', p: 13500, c: 'postres', d: 'Color vibrante y sabor aterciopelado.', img: 'https://images.unsplash.com/photo-1616541823729-00fe0aacd32c?w=400' },
  { id: 27, n: 'Muffin Arándanos', p: 7000, c: 'postres', d: 'Esponjoso y cargado de fruta.', img: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSphR3FT41Ut3xYLXZuwEDBIdQKy4klR0i2og&s' },
  { id: 28, n: 'Crepe de Arequipe', p: 13000, c: 'postres', d: 'Con doble ración de dulce de leche.', img: 'https://images.unsplash.com/photo-1519676867240-f03562e64548?w=400' },
  { id: 29, n: 'Profiteroles', p: 12500, c: 'postres', d: 'Rellenos de crema y bañados en choco.', img: 'https://images.unsplash.com/photo-1505976378723-9726b54e9bb9?w=400' },
  { id: 30, n: 'Apple Pie', p: 11500, c: 'postres', d: 'Manzanas canela en masa hojaldrada.', img: 'https://images.unsplash.com/photo-1568571780765-9276ac8b75a2?w=400' }
]);

// LÓGICA
setTimeout(() => { cargando.value = false; }, 800);

const añadir = (p) => {
  carrito.value.push(p);
  total.value += p.p;
};

const quitar = (idx) => {
  total.value -= carrito.value[idx].p;
  carrito.value.splice(idx, 1);
};

const confirmar = () => {
  if (carrito.value.length > 0) verConfirmacion.value = true;
};

const limpiar = () => {
  carrito.value = [];
  total.value = 0;
  verConfirmacion.value = false;
};
</script>

<template>
  <div class="main-app">
    
    <div v-if="cargando" class="full-loader">
      <div class="logo-anim">A</div>
      <p>AMALIA</p>
    </div>

    <header class="navbar">
      <div class="brand">
        <h1>🗿 GELATO</h1>
        <span>Café & Gelato</span>
      </div>
      <div class="tabs">
        <button @click="categoria = 'helados'" :class="{active: categoria === 'helados'}">Helados</button>
        <button @click="categoria = 'cafes'" :class="{active: categoria === 'cafes'}">Cafetería</button>
        <button @click="categoria = 'postres'" :class="{active: categoria === 'postres'}">Postres</button>
      </div>
    </header>

    <div class="content">
      
      <aside class="cart-sidebar">
        <div class="cart-box">
          <div class="cart-header">
            <h3>TU SELECCIÓN</h3>
            <span class="count">{{ carrito.length }} items</span>
          </div>
          
          <div class="cart-list">
            <p v-if="carrito.length === 0" class="empty-txt">Elige algo delicioso del menú</p>
            <div v-for="(item, i) in carrito" :key="i" class="cart-item">
              <div class="item-info">
                <strong>{{ item.n }}</strong>
                <p>${{ item.p.toLocaleString() }}</p>
              </div>
              <button @click="quitar(i)" class="del-btn">✕</button>
            </div>
          </div>

          <div class="cart-footer">
            <div class="total-row">
              <span>Total a pagar</span>
              <strong>${{ total.toLocaleString() }}</strong>
            </div>
            <button @click="confirmar" class="confirm-btn">PEDIR AHORA</button>
          </div>
        </div>
      </aside>

      <section class="menu-display">
        <h2 class="category-name">Menú: {{ categoria }}</h2>
        <div class="rows-container">
          <template v-for="p in lista" :key="p.id">
            <div v-if="p.c === categoria" class="product-row">
              <div class="img-wrapper">
                <img :src="p.img" alt="">
              </div>
              <div class="details">
                <div class="title-price">
                  <h4>{{ p.n }}</h4>
                  <span class="tag">${{ p.p.toLocaleString() }}</span>
                </div>
                <p class="description">{{ p.d }}</p>
                <button @click="añadir(p)" class="add-row-btn">AÑADIR AL CARRITO</button>
              </div>
            </div>
          </template>
        </div>
      </section>

    </div>

    <div v-if="verConfirmacion" class="overlay">
      <div class="success-card">
        <div class="success-icon">✓</div>
        <h2>¡Orden Recibida!</h2>
        <p>Estamos preparando tu pedido en Amalia.</p>
        
        <div class="order-details-box">
          <div class="order-scroll">
            <div v-for="(i, idx) in carrito" :key="idx" class="summary-line">
              <span>{{ i.n }}</span>
              <span>${{ i.p.toLocaleString() }}</span>
            </div>
          </div>
          <div class="final-total">
            <span>TOTAL CANCELADO</span>
            <span>${{ total.toLocaleString() }}</span>
          </div>
        </div>

        <button @click="limpiar" class="done-btn">VOLVER AL MENÚ</button>
      </div>
    </div>

  </div>
</template>