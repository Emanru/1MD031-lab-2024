<template>
  <div class="burger">
    <h1>{{ burger.name }}</h1>
    <img v-bind:src="burger.img">
    <p>{{ burger.kCal }} kCal</p>
    <p v-if="burger.gluten == true || burger.lactose == true">Allergens: </p>
    <ul class="allergens">
      <li v-if="burger.gluten == true">Gluten</li>
      <li v-if="burger.lactose == true">Lactose</li>
    </ul>
    <button v-on:click="addBurger">Add</button>
    <button v-on:click="removeBurger">Remove</button>
    <p>{{ amountOrdered }}</p>
  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
  data: function () {
    return {
      amountOrdered: 0,
    }
  },
  methods: {
    addBurger: function() {
      this.amountOrdered++;
    },
    removeBurger: function() {
      this.amountOrdered--;
    }
  }
}
</script>

<style scoped>
h1 {
  font-size: 16pt;
}

.burger {
    margin: 5px;
    padding: 5px;
}

.burger img {
    width:100%;
    height:auto;
}
</style>