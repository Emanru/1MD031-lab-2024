<template>
  <header>
    <h1>Welcome to All Things Burger!</h1>
  </header>
  <main>
    <section class = "burgersection">
      <h2>Select burger</h2>
      <p>This is where you execute burger selection</p>
      <div class="burger-wrapper">
        <Burger v-for="burger in burgers"
                v-bind:burger="burger"
                v-bind:key="burger.name"
                v-on:orderedBurger="addToOrder($event)"/>
      </div>
    </section>
    <section class="contactsection">
      <form>
        <h2>Customer Information</h2>
        <p>This is where you provide necessary information</p>
        <h2>Delivery Information</h2>
        <p>
          <label for="fullname">Full name</label><br>
          <input type="text" v-model="fullname" id="fullname" required="required" placeholder="First- and Last name">
        </p>
        <p>
          <label for="email">E-mail</label><br>
          <input type="email" v-model="email" id="email" required="required" placeholder="E-mail adress">
        </p>
        <p>
          <label for="street">Street</label><br>
          <input type="text" v-model="street" id="street" required="required" placeholder="Street name">
        </p>
        <p>
          <label for="house">House</label><br>
          <input type="number" v-model="house" id="house" name="fn" required="required" placeholder="House number">
        </p>
        <p>
          <label for="payment">Payment options</label><br>
          <select v-model="payment" id="payment">
            <option selected="selected">Credit card</option>
            <option>Swish</option>
            <option>Klarna</option>
            <option>PayPal</option>
            <option>Cash</option>
          </select>
        </p>
        <p>Gender</p>
        <p>
          <input type="radio" v-model="gender" id="male" value="male">
          <label for="male">Male</label>
        </p>
        <p>
          <input type="radio" v-model="gender" id="female" value="female">
          <label for="female">Female</label>
        </p>
        <p>
          <input type="radio" v-model="gender" id="other" value="other">
          <label for="other">Other</label>
        </p>
        <p>
          <input type="radio" v-model="gender" id="undisclosed" value="undisclosed" checked>
          <label for="undisclosed">Undisclosed</label>
        </p>
      </form>
    </section>
  </main>
  <button v-on:click="placeOrder()" type="submit">
    <img src="/img/delivery.png">
    Place my order!
  </button>
  <hr>
  <footer>
    &copy; Emanru Creative Group Inc.
  </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io("localhost:3000");

function MenuItem(name, url, kCal, glu, lac) {
  this.name = name;
  this.img = url;
  this.kCal = kCal;
  this.gluten = glu;
  this.lactose = lac;
}

const burgerArray = [menu[0], menu[1], menu[2]]

/* const burgerArray = [new MenuItem("Fireburger", "/img/fireburger.jpg", 764, true, true),
                 new MenuItem("Burgerstack", "/img/stackofburgers.jpg", 2292, true, true),
                 new MenuItem("Horseburger", "/img/horseburger.jpg", 468, true, false)
                ]
*/

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: burgerArray,
      fullname: '',
      email: '',
      street: '',
      house: '',
      payment: '',
      gender: 'undisclosed',
      orderedBurgers:{},
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    },
    addToOrder: function(event) {
      this.orderedBurgers[event.name] = event.amount;
    },
    placeOrder: function () {
      console.log(this.fullname, this.email, this.street, 
                  this.house, this.payment, this.gender,
                  this.orderedBurgers);
    }
  }
}
</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }

  @import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');

  header {
      background-image: url("/img/headerimg.png");
      background-size: cover;
      overflow: hidden;
      width: 100%;
      height: 200px;
      margin: 10px;
      opacity: 0.5;
  }

  header h1 {
      width: 40rem;
      margin: 50px auto;
      text-align: center;
      color: black;
  }


  body {
      font-family: 'Times New Roman', Times, serif;
      font-size: 16pt;
  }

  h2 {
      font-family:'Times New Roman', Times, serif;
      font-size: 20pt;
  }

  .burgersection {
      background-color: black;
      color:white;
      border: 4px dashed white;
      padding: 5px;
      margin: 10px;
  }

  .burger-wrapper {
      display: grid;
      grid-gap: 10px;
      grid-template-columns: 25% 25% 25%;
      margin: 10px;
  }

  .burger {
      margin: 5px;
      padding: 5px;
  }

  .burger img {
      width:100%;
      height:auto;
  }

  .ingredients {
      font-weight: bold;
      margin: auto 5px;
  }

  .contactsection {
      background-color: white;
      margin: 10px;
      padding: 10px;
      border: 4px dashed black;
  }

  button {
      margin: 10px;
  }

  button img {
      height: 30px;
      width: 30px;
      margin-top: 5px;
      margin-bottom: 5px;
      margin-left: 0px;
  }

  button:hover {
      background-color: rgb(93, 152, 93);
      cursor: pointer;
  }

  button:active {
      background-color: rgb(48, 89, 60);
  }

  footer {
      font-family: Arial, Helvetica, sans-serif;
      font-size: 10pt;
  }
</style>