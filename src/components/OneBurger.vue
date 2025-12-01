<template>
    <div class="burger-item">
        <h3>{{ burger.name }}</h3>
        <img :src="burger.url" :alt="burger.name">

        <ul>
            <li>KCal: {{ burger.kCal }}</li>
            <li v-if="burger.gluten">Contains <span class="allergen">gluten</span></li>
            <li v-if="burger.lactose">Contains <span class="allergen">lactose</span></li>

        </ul>

        <div class="quantity">
            <button @click="decrease">-</button>
            <span>{{ amountOrdered }}</span>
            <button @click="addBurger">+</button>
        </div>

    </div>
</template>

<script>
export default {
    name: 'OneBurger',
    props: {
        burger: Object
    },

    data() {
        return {
            amountOrdered: 0
        }
    },

    methods: {
        decrease() {
            if (this.amountOrdered > 0) {
                this.amountOrdered--
            }
        },

        addBurger() {
           this.amountOrdered += 1;
           this.$emit('orderedBurger', {
            name: this.burger.name,
            amount: this.amountOrdered
    });
},

    }
}
</script>

<style scoped>
.burger-item {
    padding: 10px;
    text-align: left;
}

.burger-item img {
    width: 100%;
    height: auto;
    display: block;
}

.allergen {
    font-weight: bold;
    color: darkred;
}

.quantity {
    margin-top: 10px;
}

.quantity button {
    padding: 5px;
    margin: 0 5px;
}
</style>
