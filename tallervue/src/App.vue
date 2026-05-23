<script setup>
import { ref } from 'vue'
import { jsPDF } from "jspdf"

// ESTADOS
const cargando = ref(true)
const categoria = ref('helados')
const carrito = ref([])
const total = ref(0)
const verConfirmacion = ref(false)

// PRODUCTOS
const lista = ref([

  // =========================
  // HELADOS (10)
  // =========================
  { id: 1, n: 'Copa Amalia Suprema', p: 24500, c: 'helados', d: 'Tres bolas premium con salsa artesanal.', img: 'https://images.unsplash.com/photo-1563805042-7684c019e1cb?w=400' },

  { id: 2, n: 'Bola Vainilla Bourbon', p: 11000, c: 'helados', d: 'Vainilla natural de madagascar.', img: 'https://images.unsplash.com/photo-1570197788417-0e82375c9371?w=400' },

  { id: 3, n: 'Sorbet de Mango-Biche', p: 12000, c: 'helados', d: 'Refrescante con un toque de sal y limón.', img: 'https://images.unsplash.com/photo-1534706936160-d5ee67737249?w=400' },

  { id: 4, n: 'Gelato de Avellana', p: 16000, c: 'helados', d: 'Textura cremosa con avellanas tostadas.', img: 'https://images.unsplash.com/photo-1580915411954-282cb1b0d780?w=400' },

  { id: 5, n: 'Malteada de Caramelo', p: 18500, c: 'helados', d: 'Batido denso con fudge de caramelo.', img: 'https://www.cocinadelirante.com/800x600/filters:format(webp):quality(75)/sites/default/files/images/2024/10/malteada-caramelo.jpg' },

  { id: 6, n: 'Sundae de Chocolate', p: 15500, c: 'helados', d: 'Con brownie troceado y sirope caliente.', img: 'https://images.unsplash.com/photo-1579954115545-a95591f28bfc?w=400' },

  { id: 7, n: 'Cono Especial Kids', p: 8500, c: 'helados', d: 'Sabor chicle con chispas de colores.', img: 'https://images.unsplash.com/photo-1516559828984-fb3b99548b21?w=400' },

  { id: 8, n: 'Nieve de Maracuyá', p: 10500, c: 'helados', d: '100% pulpa de fruta natural.', img: 'https://images.unsplash.com/photo-1497034825429-c343d7c6a68f?w=400' },

  { id: 9, n: 'Paleta de Pistacho', p: 9500, c: 'helados', d: 'Rellena de crema de pistacho.', img: 'https://images.unsplash.com/photo-1567206563064-6f60f40a2b57?w=400' },

  { id: 10, n: 'Banana Split Amalia', p: 27000, c: 'helados', d: 'El clásico con un toque gourmet.', img: 'https://images.unsplash.com/photo-1576506295286-5cda18df43e7?w=400' },

  // =========================
  // CAFÉS (10)
  // =========================
  { id: 11, n: 'Flat White', p: 9800, c: 'cafes', d: 'Doble espresso con leche micro-cremosa.', img: 'https://images.unsplash.com/photo-1495474472287-4d71bcdd2085?w=400' },

  { id: 12, n: 'Latte de Vainilla', p: 11500, c: 'cafes', d: 'Suave y dulce para cualquier hora.', img: 'https://images.unsplash.com/photo-1511920170033-f8396924c348?w=400' },

  { id: 13, n: 'Cappuccino Tradicional', p: 9000, c: 'cafes', d: 'Equilibrio perfecto entre café y espuma.', img: 'https://images.unsplash.com/photo-1534778101976-62847782c213?w=400' },

  { id: 14, n: 'Espresso Sencillo', p: 5500, c: 'cafes', d: 'Energía pura en una taza pequeña.', img: 'https://images.unsplash.com/photo-1510591509098-f4fdc6d0ff04?w=400' },

  { id: 15, n: 'Mocaccino Dark', p: 12500, c: 'cafes', d: 'Café con chocolate amargo al 70%.', img: 'https://images.unsplash.com/photo-1610889556528-9a770e32642f?w=400' },

  { id: 16, n: 'Affogato Espresso', p: 14000, c: 'cafes', d: 'El puente entre helado y café.', img: 'https://images.unsplash.com/photo-1517701550927-30cf4ba1f0f3?w=400' },

  { id: 17, n: 'Cold Brew Citrus', p: 11000, c: 'cafes', d: 'Café frío con rodajas de naranja.', img: 'https://images.unsplash.com/photo-1461023058943-07fcbe16d735?w=400' },

  { id: 18, n: 'Americano de Origen', p: 6000, c: 'cafes', d: 'Café suave con notas frutales.', img: 'https://images.unsplash.com/photo-1509042239860-f550ce710b93?w=400' },

  { id: 19, n: 'Frappé Cookies', p: 16500, c: 'cafes', d: 'Granizado de café con galleta oreo.', img: 'https://images.unsplash.com/photo-1572490122747-3968b75cc699?w=400' },

  { id: 20, n: 'Té Chai Latte', p: 12000, c: 'cafes', d: 'Mezcla de especias y leche caliente.', img: 'https://images.unsplash.com/photo-1579888944880-d98341245702?w=400' },

  // =========================
  // POSTRES (10)
  // =========================
  { id: 21, n: 'Tiramisú Casero', p: 15000, c: 'postres', d: 'Bizcocho empapado en café y mascarpone.', img: 'https://images.unsplash.com/photo-1571877227200-a0d98ea607e9?w=400' },

  { id: 22, n: 'Cheesecake Frutos', p: 14500, c: 'postres', d: 'Tarta de queso horneada con mermelada.', img: 'https://images.unsplash.com/photo-1533134242443-d4fd215305ad p: 14500', img: 'https://images.unsplash.com/photo-1533134242443-d4fd215305ad?w=400' },

  { id: 23, n: 'Waffle con Nutella', p: 17000, c: 'postres', d: 'Recién hecho, crujiente y dulce.', img: 'https://images.unsplash.com/photo-1519676867240-f03562e64548?w=400' },

  { id: 24, n: 'Brownie de la Casa', p: 8000, c: 'postres', d: 'Chocolate intenso con nueces.', img: 'https://images.unsplash.com/photo-1606313564200-e75d5e30476c?w=400' },

  { id: 25, n: 'Pie de Limón', p: 11000, c: 'postres', d: 'Equilibrio cítrico con merengue suizo.', img: 'https://images.unsplash.com/photo-1464306076886-da185f6a9d05?w=400' },

  { id: 26, n: 'Torta Red Velvet', p: 13500, c: 'postres', d: 'Color vibrante y sabor aterciopelado.', img: 'https://images.unsplash.com/photo-1586788680434-30d324b2d46f?w=400' },

  { id: 27, n: 'Muffin Arándanos', p: 7000, c: 'postres', d: 'Esponjoso y cargado de fruta.', img: 'https://images.unsplash.com/photo-1607958996333-41aef7caefaa?w=400' },

  { id: 28, n: 'Crepe de Arequipe', p: 13000, c: 'postres', d: 'Con doble ración de dulce de leche.', img: 'https://images.unsplash.com/photo-1488477181946-6428a0291777?w=400' },

  { id: 29, n: 'Profiteroles', p: 12500, c: 'postres', d: 'Rellenos de crema y bañados en chocolate.', img: 'https://images.unsplash.com/photo-1488477304112-4944851de03d?w=400' },

  { id: 30, n: 'Apple Pie', p: 11500, c: 'postres', d: 'Manzanas canela en masa hojaldrada.', img: 'https://images.unsplash.com/photo-1568571780765-9276ac8b75a2?w=400' }
])

// LOADER
setTimeout(() => {
  cargando.value = false
}, 800)

// AÑADIR
const añadir = (p) => {
  carrito.value.push(p)
  total.value += p.p
}

// QUITAR
const quitar = (idx) => {
  total.value -= carrito.value[idx].p
  carrito.value.splice(idx, 1)
}

// CONFIRMAR PEDIDO + PDF
const confirmar = () => {
  if (carrito.value.length === 0) {
    alert("El carrito está vacío")
    return
  }
  verConfirmacion.value = true

  const doc = new jsPDF()
  doc.setFontSize(24)
  doc.text("AMALIA GELATO", 20, 20)
  doc.setFontSize(13)
  doc.text("Factura de compra", 20, 30)
  const fecha = new Date().toLocaleString()
  doc.text(`Fecha: ${fecha}`, 20, 40)
  doc.line(20, 45, 190, 45)

  let y = 55
  carrito.value.forEach((item, index) => {
    doc.text(`${index + 1}. ${item.n} - $${item.p.toLocaleString()}`, 20, y)
    y += 10
  })

  doc.line(20, y, 190, y)
  y += 10
  doc.setFontSize(15)
  doc.text(`TOTAL PAGADO: $${total.value.toLocaleString()}`, 20, y)
  y += 20
  doc.setFontSize(11)
  doc.text("Gracias por comprar en Amalia Gelato ☕🍨", 20, y)
  doc.save("factura-amalia.pdf")
}

// LIMPIAR
const limpiar = () => {
  carrito.value = []
  total.value = 0
  verConfirmacion.value = false
}
</script>

<template>
  <div class="main-app">
    <!-- LOADER -->
    <div v-if="cargando" class="full-loader">
      <div class="logo-anim">A</div>
      <p>AMALIA</p>
    </div>

    <!-- NAVBAR -->
    <header class="navbar">
      <div class="brand">
        <h1>🗿 GELATO</h1>
        <span>Café & Gelato</span>
      </div>

      <div class="tabs">
        <button
          @click="categoria = 'helados'"
          :class="{active: categoria === 'helados'}"
        >
          Helados
        </button>

        <button
          @click="categoria = 'cafes'"
          :class="{active: categoria === 'cafes'}"
        >
          Cafetería
        </button>

        <button
          @click="categoria = 'postres'"
          :class="{active: categoria === 'postres'}"
        >
          Postres
        </button>
      </div>
    </header>

    <!-- CONTENIDO -->
    <div class="content">
      <!-- CARRITO -->
      <aside class="cart-sidebar">
        <div class="cart-box">
          <div class="cart-header">
            <h3>TU SELECCIÓN</h3>
            <span class="count">{{ carrito.length }} items</span>
          </div>

          <div class="cart-list">
            <p v-if="carrito.length === 0" class="empty-txt">
              Elige algo delicioso del menú
            </p>

            <div
              v-for="(item, i) in carrito"
              :key="i"
              class="cart-item"
            >
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

            <button @click="confirmar" class="confirm-btn">
              PEDIR AHORA
            </button>
          </div>
        </div>
      </aside>

      <!-- PRODUCTOS -->
      <section class="menu-display">
        <h2 class="category-name">
          Menú: {{ categoria }}
        </h2>

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

                <button @click="añadir(p)" class="add-row-btn">
                  AÑADIR AL CARRITO
                </button>
              </div>
            </div>
          </template>
        </div>
      </section>
    </div>

    <!-- CONFIRMACIÓN -->
    <div v-if="verConfirmacion" class="overlay">
      <div class="success-card">
        <div class="success-icon">✓</div>
        <h2>¡Orden Recibida!</h2>
        <p>Estamos preparando tu pedido en Amalia.</p>

        <div class="order-details-box">
          <div class="order-scroll">
            <div
              v-for="(i, idx) in carrito"
              :key="idx"
              class="summary-line"
            >
              <span>{{ i.n }}</span>
              <span>${{ i.p.toLocaleString() }}</span>
            </div>
          </div>

          <div class="final-total">
            <span>TOTAL CANCELADO</span>
            <span>${{ total.toLocaleString() }}</span>
          </div>
        </div>

        <button @click="limpiar" class="done-btn">
          VOLVER AL MENÚ
        </button>
      </div>
    </div>
  </div>
</template>
