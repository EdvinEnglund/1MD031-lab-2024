<template>
  <header>
    <img src="/img/many.png" id="top" />
    <h1 id="hello">Prince of burgers</h1> <!-- this is how u comment code-->
  </header>

  <section id="burger">
    <h2>Select the burger of your choice</h2>
    <p>All of our burgers are made on order from 100% grass-fed beef (or wild-caught fish)</p>
    <section class="wrapper">
      <Burger
        v-for="burger in burgers"
        v-bind:burger="burger"
        v-bind:key="burger.name"
        v-on:orderedBurger="addToOrder($event)"
      />
    </section>
  </section>

  <section id="delivery">
    <h2>Delivery details</h2>
    <p>As of now, we only deliver to addresses in Uppsala, Sweden</p>
    <form>
      <p>
        <label for="fullname">Full Name</label><br />
        <input
          v-model="fullname"
          id="fullname"
          required="required"
          placeholder="First and last name"
        />
      </p>
      <p>
        <label for="email">E-mail</label><br />
        <input
          v-model="email"
          id="email"
          required="required"
          placeholder="E-mail address"
        />
      </p>
      <p>Click on the map below to select delivery location:</p>
      <div id="mapWrap">
        <div
          id="map"
          v-on:click="setLocation"
          style="position: relative;"
        >
          <div
            v-bind:style="{ 
              position: 'absolute', 
              left: location.x + 'px', 
              top: location.y + 'px', 
              width: '10px', 
              height: '10px' 
            }"
          >
            T
          </div>
        </div>
      </div>
      <p>
        <label for="method">Payment method</label><br />
        <select id="method" v-model="paymentMethod">
          <option>Credit/Debit Card</option>
          <option>Swish</option>
          <option>Cash</option>
        </select>
      </p>
      <p>Your gender</p>
      <input
        type="radio"
        id="man"
        name="gender"
        value="man"
        v-model="picked"
      />
      <label for="man">Man</label><br />
      <input
        type="radio"
        id="woman"
        name="gender"
        value="woman"
        v-model="picked"
      />
      <label for="woman">Woman</label><br />
      <input
        type="radio"
        id="Other"
        name="gender"
        value="other"
        v-model="picked"
      />
      <label for="other">Other</label><br />
      <input
        type="radio"
        id="none"
        name="gender"
        value="none"
        v-model="picked"
      />
      <label for="none">None of your business</label><br />
    </form>
    <button type="submit" v-on:click="addOrder">
      <img src="/img/shop.png" style="width:20px" />
      Place order
    </button>
  </section>
  <hr />
  <footer>
    &copy; 2024 Prince Desiderius Burgers AB
  </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue';
import io from 'socket.io-client';

const socket = io("localhost:3000");

class MenuItem {
  constructor(name, kCal, gluten, lactose, image) {
    this.name = name;
    this.kCal = kCal;
    this.gluten = gluten;
    this.lactose = lactose;
    this.image = image;
  }
}

import menu from '../assets/menu.json';
console.log(menu);

export default {
  name: 'HomeView',
  components: {
    Burger,
  },
  data: function () {
    return {
      burgers: menu,
      orderedBurgers: {},
      location: { x: 0, y: 0 },
    };
  },
  methods: {
    setLocation(event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };
      this.location = {
        x: event.clientX - 10 - offset.x,
        y: event.clientY - 10 - offset.y,
      };
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
      console.log("orderedBurger", this.orderedBurgers);
    },
    printOrderData() {
      console.log("Full Name:", this.fullname);
      console.log("Email:", this.email);
      console.log("Payment Method:", this.paymentMethod);
      console.log("Gender:", this.picked);
    },
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    addOrder: function (event) {
      console.log(this.orderedBurgers);
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        orderItems: this.orderedBurgers,
        details: {
          x: this.location.x,
          y: this.location.y,
          fullName: this.fullname,
          email: this.email,
          gender: this.picked,
          paymentMethod: this.paymentMethod,
        },
      });
    },
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
/* Your comment goes here */
.wrapper {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: 33% 33% 33%;
  background-color: rgb(255, 199, 121);
  color: #444;
  border-radius: 50px;
}

.box {
  background-color: #000;
  color: #fff;
  border-radius: 5px;
  padding: 20px;
  font-size: 100%;
  border-radius: 50px;
}
.box:hover {
  color: #ff7000;
}

section {
  margin: 50px;
}
#top {
  opacity: 0.5;
  width: 100%;
  height: auto;
}
#hello {
  position: absolute;
  margin-top: -950px;
  margin-left: 20px;
  color: #fff;
}
header {
  margin: 0px 75px;
  overflow: hidden;
  height: 200px;
  font-size: 36pt;
}
button {
  margin: 50px 0px;
}
body {
  font-size: 12pt;
  font-family: Arial;
}

.ingredients {
  color: #ff7000;
}

#burger {
  color: #000;
  background-color: rgb(255, 199, 121);
  border-style: dotted;
  border-color: #000;
  border-radius: 50px;
}

#burger h2,
#burger p {
  margin: 20px;
}
#delivery {
  padding: 20px;
  border-color: black;
  border-style: dotted;
  border-radius: 50px;
}
button:hover {
  background-color: #ff7000;
  color: white;
}

#map {
  background: url("/img/polacks.jpg");
  width: 1920px;
  height: 1078px;
}
#mapWrap {
  overflow: scroll;
  height: 400px;
  width: 400px;
}
</style>
