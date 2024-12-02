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
      <p>Please select destination</p>
      <div class="mapwrapper">
        <div id="map" v-on:click="setLocation">
          <div v-bind:style="{left: location.x + 'px', top: location.y + 'px'}">
            T
          </div>
        </div>
      </div>
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
      payment: '',
      gender: 'undisclosed',
      location: {x: 0,
                 y: 0
                },
      orderedBurgers:{}
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {},
    addToOrder: function(event) {
      this.orderedBurgers[event.name] = `${event.name}`+` (${event.amount})`;
    },
    placeOrder: function () {
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                location: { x: this.location.x,
                                           y: this.location.y,
                                          },
                                info: {name: this.fullname,
                                       email: this.email,
                                       payment: this.payment,
                                       gender: this.gender
                                      },           
                                orderItems: this.orderedBurgers
                              }
                 );
      console.log(this.fullname, this.email, this.payment, 
                  this.gender, this.orderedBurgers);
    },
    setLocation: function(event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y;
    }

  }
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
  
  header {
      background-image: url("/img/headerimg.png");
      background-size: cover;
      overflow: hidden;
      width: 100%;
      height: 200px;
      opacity: 0.5;
  }

  header h1 {
      position: relative;
      width: 40rem;
      margin: 50px auto;
      text-align: center;
      color: black;
      font-size: 36pt;
      opacity: 1.0 !important;
  }


  main {
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

  .mapwrapper {
      height: 300px;
      margin: 10px;
      overflow: scroll;
  }

  #map {
      position: relative;
      margin: 0;
      padding: 0;
      background-repeat: no-repeat;
      width: 1920px;
      height: 1078px;
      cursor: crosshair;
      background: url("/img/polacks.jpg");
      
  }

  #map div {
    position: absolute;
    background-color: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
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