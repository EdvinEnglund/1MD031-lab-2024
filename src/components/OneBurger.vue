<template>
  <div class="menuItem">
    <h3>The {{ burger.name }} burger</h3>
    <img v-bind:src="burger.image" class="pic" />
    <div class="order">
      Order amount: {{ amountOrdered }}
      <button type="decrease" v-on:click="this.decreaseBurger">-</button>
      <button type="add" v-on:click="this.addBurger">+</button>
    </div>
    <ul class="ingredients">
      <li>{{ burger.kCal }} kCal</li>
      <li class="alergens" v-if="burger.gluten">Gluten</li>
      <li class="alergens" v-if="burger.lactose">Lactose</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object,
  },
  data: function () {
    return {
      amountOrdered: 0,
    };
  },
  methods: {
    decreaseBurger() {
      if (this.amountOrdered > 0) {
        this.amountOrdered--;
        this.$emit('orderedBurger', {
          name: this.burger.name,
          amount: this.amountOrdered,
        });
      }
    },
    addBurger() {
      this.amountOrdered++;
      this.$emit('orderedBurger', {
        name: this.burger.name,
        amount: this.amountOrdered,
      });
    },
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');

/* Image styles */
.pic {
  width: 440px;
  height: 440px;
}

/* Menu item styles */
.menuItem {
  background-color: #000;
  color: #fff;
  border-radius: 50px;
  padding: 20px;
  font-size: 100%;
}

.menuItem:hover {
  color: #ff7000;
}

/* Allergens styling */
.alergens {
  font-weight: bold;
}
</style>
