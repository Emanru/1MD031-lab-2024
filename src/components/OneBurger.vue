<template>
  <div class="burger">
    <h1>{{ burger.name }}</h1>
    <img v-bind:src="burger.img">
    <p>{{ burger.kCal }} kCal</p>
    <p v-if="burger.gluten == true || burger.lactose == true || burger.squid == true">Allergens: </p>
    <ul class="allergens">
      <li v-if="burger.gluten == true">Gluten</li>
      <li v-if="burger.lactose == true">Lactose</li>
      <li v-if="burger.squid == true">Squid</li>
    </ul>
    <p class="buttonlabel">Amount:
       <button v-on:click="addBurger">&plus;</button>
        {{ amountOrdered }}
       <button v-on:click="removeBurger">&minus;</button>
    </p>
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
      this.$emit('orderedBurger', {name: this.burger.name,
                                   amount: this.amountOrdered
                                  }
      );
    },
    removeBurger: function() {
      if(this.amountOrdered > 0) {
         this.amountOrdered--;
         this.$emit('orderedBurger', {name: this.burger.name,
                                      amount: this.amountOrdered
                                      }
        );
      }
      
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
    height:25%;
}

.buttonlabel {
  font-family: 'Arial', sans-serif;
  font-size: 10pt;
}

.buttonlabel button {
  height: 20px;
  width: 22px;
  text-align: center;
}

</style>