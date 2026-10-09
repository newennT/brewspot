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

      <MenuItem
        v-for="item in simpleMenu"
        :key="item.name"
        :name="item.name"
        :quantity="item.quantity"
        :cost="item.cost"
        :stock="item.stock"
        @add-to-cart="addToShoppingCart"
      />
    </section>

    <aside class="shopping-cart">
      <h2>Panier : {{ shoppingCartCount }} articles - {{ shoppingCartTotal.toFixed(2) }}€</h2>
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
import MenuItem from './components/MenuItem.vue'

export default {
  components: {
    MenuItem
  },
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
          stock: 10,
          quantity: 0,
          cost: 3
        },
        {
          name: 'Baguette',
          stock: 10,
          quantity: 0,
          cost: 3
        },
        {
          name: 'Pain au chocolat',
          stock: 5,
          quantity: 0,
          cost: 4
        }
      ],

      shoppingCart: [],

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
    },
     shoppingCartCount() {
      return this.shoppingCart.reduce(
        (total, item) => total + item.quantity,
        0
      )
    },

    shoppingCartTotal() {
      return this.shoppingCart.reduce(
        (total, item) => total + item.quantity * item.cost,
        0
      )
    }
  },

  methods: {
    addToShoppingCart({ name, quantity }) {
      const item = this.simpleMenu.find(
        product => product.name === name
      )

      if(!item || !Number.isInteger(quantity) || quantity <= 0 || quantity > item.stock) return

      item.stock -= quantity
      
      const cartItem = this.shoppingCart.find(
        product => product.name === name
      )

      if (cartItem) {
        cartItem.quantity += quantity
      } else {
        this.shoppingCart.push({
          name: item.name,
          quantity: quantity,
          cost: item.cost
        })
      }
    },

   
  }
}
</script>