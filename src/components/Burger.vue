<template>

  <div class="hej">
    <h3 id="burname">{{ burger.name }}</h3>
    <img v-bind:src="burger.img" alt="span" style="height: 200px; border-radius: 30px;
         border-radius: 20px; border: 3px solid green">
    <ul>
      <li>{{ burger.kCal }} kCal <span id="big" v-if="burger.kCal>=700"> BIG MEAL</span> </li>
      <span v-if="burger.lactose == true"> <li>Contains <span class="allergies">lactose</span></li></span>
      <span v-if="burger.gluten == true"> <li>Contains <span class="allergies">gluten</span></li></span>
    </ul>
    <div id="orderButton">
    <span><button v-on:click="increase($event)" >Add</button>
    </span>
    <span><button v-on:click="decrease($event)">Remove</button>
    <img src="https://icon-library.com/images/cart-icon-png-white/cart-icon-png-white-11.jpg" style="width: 25px">{{amountOrdered}}</span>
    </div>


  </div>
</template>

<script>

export default {

  name: 'Burger',
  props: {
    burger: Object
  },

  data: function () {
    return {
      amountOrdered: 0,

    }
  },

  methods: {
    increase: function () {
      this.amountOrdered ++;
      console.log(this.burger.name, this.amountOrdered)
      this.$emit('orderedBurger', {name: this.burger.name,
        amount: this.amountOrdered});
    },
    decrease: function () {
      if (this.amountOrdered > 0) {
        this.amountOrdered--;
      }
      this.$emit('orderedBurger', {name: this.burger.name,
        amount: this.amountOrdered});
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>


#burname{
  align-content: center;

}
.hej{
  margin-left: 50px;
}

.allergies{
  font-weight: bold;
  text-transform: uppercase;
}
#big {
  color: #9f100c;
}
#orderButton{
  margin-top: -15px;
  color: white;
}

</style>
