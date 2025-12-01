

<template>
<header>
        <h1>Welcome to Burgers Online</h1>
        <img src="https://studyinsweden.se/transformations/2017/10/bar-2209813_1920-1.jpg-992x.jpg"
            alt="restaurant interior">
    </header>

    <main>


        <section id="burgers">

            <h2>Select burger</h2>
            <p>This is where you execute burger selection</p>

            <div class="burger-grid">

              <Burger 
               v-for="burger in burgers"
               v-bind:burger="burger"
               v-bind:key="burger.name"
               v-on:orderedBurger="addToOrder($event)"/>
              

            </div>

        </section>




<section id="customer">

  <h2>Customer information</h2>
  <p>This is where you provide necessary information</p>

  <h3>Delivery information:</h3>

  <form>
    <p>
      <label for="fullname">Full name</label><br>
      <input 
        type="text" 
        id="fullname" 
        placeholder="First- and Last name"
        v-model="fullname"
      >
    </p>

    <p>
      <label for="email">E-mail</label><br>
      <input 
        type="email" 
        id="email"
        placeholder="E-mail address"
        v-model="email"
      >
    </p>


    <p>
      <label for="payment">Payment method</label><br>
      <select id="payment" v-model="payment">
        <option value="card">Credit Card</option>
        <option value="swish">Swish</option>
        <option value="paypal">PayPal</option>
        <option value="klarna">Klarna</option>
      </select>
    </p>

    <p>
      <label>Gender</label><br>

      <input type="radio" id="male" value="male" v-model="gender">
      <label for="male">Male</label><br>

      <input type="radio" id="female" value="female" v-model="gender">
      <label for="female">Female</label><br>

      <input type="radio" id="noinfo" value="noinfo" v-model="gender">
      <label for="noinfo">Do not wish to provide</label>
    </p>

Point Delivery
<div id="map-wrapper">
  <div id="map" @click="setLocation">
    <div id="dot"
        :style="{
          left: location.x + 'px',
          top: location.y + 'px'
        }">
        T
    </div>
  </div>
</div>


  </form>

</section>



        <button type="button" v-on:click="sendOrder">
            <img src="https://cdn.pixabay.com/photo/2016/03/31/14/37/check-mark-1292787_1280.png"
           style="width:20px; vertical-align:middle; margin-right:5px;">
           Place Order
        </button>





    </main>

    <footer>
        <p>© 2025 Hypothetical Burgers Inc.</p>
    </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io("localhost:3000");

function MenuItem(name, url, kCal, hasGluten, hasLactose) {
  this.name = name;
  this.url = url;
  this.kCal = kCal;
  this.hasGluten = hasGluten;
  this.hasLactose = hasLactose;
}
const burgers = menu;

console.log(burgers)



export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      yourVariable: "Välj en burgare",
      burgers: menu,
      fullname: "",
      email: "",
      payment: "card",
      gender: "male",
      orderedBurgers: {},
      location: { x: 0, y: 0 }



    }
    
  },
methods: {
    getOrderNumber() {
        return Math.floor(Math.random() * 100000);
    },

    setLocation(event) {
        var offset = {
            x: event.currentTarget.getBoundingClientRect().left,
            y: event.currentTarget.getBoundingClientRect().top
        };

        this.location = {
            x: event.clientX - 10 - offset.x,
            y: event.clientY - 10 - offset.y
        };
    },

    sendOrder() {
        socket.emit("addOrder", {
            orderId: this.getOrderNumber(),
            details: this.location,
            orderItems: this.orderedBurgers, 
            customer: {
              name: this.fullname,
              email: this.email,
              payment: this.payment,
              gender: this.gender
            } 
        });

        console.log("Order sent to server!");
    },

    addToOrder(event) {
        this.orderedBurgers[event.name] = event.amount;
        console.log("Ordered burgers:", this.orderedBurgers);
    }
}


}

</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');

#map {
    width: 1920px;          
    height: 1078px;
    background-image: url("/img/polacks.jpg");
    background-size: cover;
    background-position: top left;
    position: relative;        
}

#map-wrapper {
    width: 600px;        
    height: 400px;       
    overflow: scroll;    
    border: 2px solid black;
    margin: 20px;
    position: relative;
}


#dot {
    width: 20px;
    height: 20px;
    background-color: red;
    border-radius: 50%;
    position: absolute;
}



body {
    font-size: 12pt;
    font-family: "comic sans ms", sans-serif;
    color: rgb(0, 0, 0);
}

#burgers {
    background-color: black;
    color: white;
    border: 2px dashed white;
    padding: 12px;
    margin-bottom: 20px;
    margin-left: 20px;
    margin-right: 20px;
}



.burger-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);  
    gap: 2rem;                               
}

.burger-item {
    padding: 10px;
    text-align: left;
}

.burger-item img {
    width: 100%;
    height: auto;
    display: block;
}



#customer {
    border: 2px dashed rgb(5, 5, 5);
    padding: 12px;
    background-color: white;
    color: black;
    margin-left: 20px;
    margin-right: 20px;
}


#customer p {
    color: black;
}



header {
    position: relative;
    height: 220px;
    overflow: hidden;

    max-width: calc(60rem - 40px);
    margin: 20px auto;
}

header img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    opacity: 0.5;
    z-index: -1;
}

header h1 {
    position: relative;
    z-index: 1;
    text-align: center;
    color: rgb(0, 0, 0);
    padding-top: 60px;
    font-size: 36px;
}

main, footer {
    color: black;
    max-width: 60rem;
    margin: 0 auto;
}


button:hover {
    background-color: rgb(100, 128, 255);
    cursor: pointer;
}

.allergen {
    font-weight: bold;
    color: darkred;
}

button {
    margin-top: 20px;
    padding: 10px 20px;
    font-size: 16px;

}
</style>