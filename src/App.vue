<template>
  <div id="app" class="app">
    <h1>{{ restaurantName }}</h1>

    <p class="description">
      Bienvenue dans notre café {{ restaurantName }}! Nous sommes réputés pour
      notre pain et nos merveilleuses pâtisseries. Faites vous plaisir dès le
      matin ou avec un goûter réconfortant. Mais attention, vous verrez qu'il
      est difficile de s'arrêter.
    </p>

    <section class="menu">
      <h2>Menu</h2>

      <div v-for="item in simpleMenu" :key="item.name">
        <div class="menu-item">
          <h3>{{ item.name }}</h3>

          <p v-if="item.inStock">En stock</p>
          <p v-else>Rupture de stock</p>

          <div>
            <label :for="`quantity-${item.name}`">
              Quantité : {{ item.quantity }}
            </label>

            <input
              :id="`quantity-${item.name}`"
              type="number"
              v-model.number="item.quantity"
              min="0"
              :max="item.stock"
            >

            <button @click="addToShoppingCart(item.quantity)">
              Ajouter au panier
            </button>
          </div>
        </div>
      </div>
    </section>

    <aside class="shopping-cart">
      <h2>Panier : {{ shoppingCart }} articles</h2>
    </aside>

    <footer>
      <p>{{ copyright }}</p>

      <ul>
        <li v-for="item in apiResponse" :key="item.id">
          <a :href="item.url">{{ item.name }}</a>
        </li>
      </ul>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      apiResponse: [
        {
          id: 1,
          name: 'GitHub',
          url: 'https://www.github.com'
        },
        {
          id: 2,
          name: 'Twitter',
          url: 'https://www.twitter.com'
        },
        {
          id: 3,
          name: 'Netlify',
          url: 'https://www.netlify.com'
        }
      ],

      simpleMenu: [
        {
          name: 'Croissant',
          inStock: true,
          stock: 10,
          quantity: 0,
          cost: 2
        },
        {
          name: 'Baguette',
          inStock: false,
          stock: 0,
          quantity: 0,
          cost: 3
        },
        {
          name: 'Pain au chocolat',
          inStock: true,
          stock: 5,
          quantity: 0,
          cost: 4
        }
      ],

      shoppingCart: 0,

      restaurantName: 'Brew Spot',
      address: "18 avenue de l'Ecluse, 35000 Rennes",
      email: 'contact@brewspot.com',
      phone: '01 23 45 67 89',
      username: '',
      password: ''
    }
  },

  computed: {
    copyright() {
      const currentYear = new Date().getFullYear()

      return `© ${this.restaurantName} ${currentYear}`
    }
  },

  methods: {
    addToShoppingCart(amount) {
      this.shoppingCart += amount
    }
  }
}
</script>