
<template>
    <div class="menu-item">
        <h3>{{ name }}</h3>
        <p v-if="stock > 0">En stock : {{ stock }}</p>
        <p v-else>Rupture de stock</p>
        <label :for="`quantity-${name}`">
            Quantité : {{ localQuantity }}
        </label>
        <input
            :id="`quantity-${name}`"
            type="number"
            v-model.number="localQuantity"
            min="0"
            :max="stock"
        />
        <button :disabled="!isQuantityValid" 
        @click="$emit('add-to-cart', { name: name, quantity: localQuantity })" > 
            Ajouter au panier 
        </button>
    </div>
</template>


<script>
export default {
    name: 'MenuItem',
    props: [ 
        'name', 
        'quantity', 
        'cost',
        'stock'
    ],
    emits: ['add-to-cart'],

    data() { return { localQuantity: this.quantity } },

    computed: {
        isQuantityValid() {
            return (
                Number.isInteger(this.localQuantity) && 
                this.localQuantity <= this.stock
            )
        }
    }
}
</script>