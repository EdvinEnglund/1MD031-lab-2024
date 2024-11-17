<template>
  <div class="menuItem">
                <h3>The {{ burger.name }} burger</h3>
                <img v-bind:src="burger.image" class="pic">

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
    burger: Object
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
/* Your comment goes here */
.pic{
  width: 440px;
  height: 440px;
}

.menuItem {
    background-color: #000;
    color: #fff;
    border-radius: 5px;
    padding: 20px;
    font-size: 100%;
    border-radius: 50px;
}
.menuItem:hover {
    color:#ff7000;
 }
 .alergens {
    font-weight: bold;
 }
</style>