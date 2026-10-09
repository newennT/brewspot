
<template>
    <div class="menu-item">
        <h3>{{ name }}</h3>
        <p v-if="inStock">En stock</p>
        <p v-else>Rupture de stock</p>
        <label :for="`quantity-${name}`">
            Quantité : {{ quantity }}
        </label>
        <input
            :id="`quantity-${name}`"
            type="number"
            v-model.number="localQuantity"
        />
        <button :disabled="!inStock || localQuantity <= 0" @click="addToShoppingCart(cost * localQuantity)" > 
            Ajouter au panier 
        </button>
    </div>
</template>


<script>
export default {
    name: 'MenuItem',
    props: ['addToShoppingCart', 'inStock', 'name', 'quantity', 'cost'],

    data() { return { localQuantity: this.quantity } }
}
</script>