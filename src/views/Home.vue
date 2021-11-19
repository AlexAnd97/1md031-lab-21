<template>
  <header>
    <div id="header">
      <img id="cover" src="https://popmenucloud.com/cdn-cgi/image/width=1200,height=1200,fit=scale-down,format=auto,quality=60/pnkdlobe/e76c1998-6dbb-44e7-ac9f-a262e9aea08e.jpg">
      <h1 id="welcome">Welcome to BurgerHeaven Online</h1>
    </div>
    <meta charset="utf-8"/>
  </header>
  <div class="choose">
    <h1>Pick your order</h1>
    <h3>Choose burgers from the menu below</h3>
  </div>
  <div class="wrapper">
    <Burger v-for="burger in burgers"
            v-bind:burger="burger" 
            v-bind:key="burger.name"
            v-on:orderedBurger="addToOrder($event)"
    />
  </div>
  <section id="information">
    <div id="infoMap">
    <form id="custinfo">
      <h2>Customer information</h2>
      <p>Please provide neccesary information </p>
      <h3>Delivery information</h3>
      <p>
        <label for="Name">Full name</label><br>
        <input type="text" id="Name" v-model="fn" required="required" placeholder="First- and last name">
      </p>
      <span>Input:{{fn}}</span>
      <p id="contact">
        <label for="email">Email</label><br>
        <input type="email" id="email" v-model="em" required="required" placeholder="Email-adress">
      </p>
      <span>Input:{{em}}</span>
      <p>
        <label for="street">Street</label><br>
        <input type="text" id="street" v-model="st" required="required" placeholder="Street name">
      </p>
      <span>Input:{{st}}</span>
      <p>
        <label for="number">House</label><br>
        <input type="number" id="number" v-model="nr" required="required" placeholder="House number">
      </p>
      <span>Input:{{nr}}</span>
      <p>
        <label>Payment options:</label><be>
        <select id="pay-options" v-model="payment">
          <option>Credit card</option>
          <option selected="selected">Swish</option>
          <option>Klarna</option>
          <option>Bank transfer</option>
        </select>
        <span>Selected: {{payment}}</span>
      </be>

      </p>
      <legend>Gender</legend>
      <input type="radio" id="male" value="male" v-model="gender">
      <label for="male">Male</label><br>
      <input type="radio" id="female" value="female" v-model="gender">
      <label for="female">Female</label><br>
      <input type="radio" id="other" value="other" v-model="gender">
      <label for="other">Other</label><br>
      <input checked="checked" type="radio" id="undisclosed" value="undisclosed" v-model="gender">
      <label for="undisclosed">Undisclosed</label><br>
      <span> Picked:{{gender}}</span>

    </form>
      <div id="location">
        <h2> Select your location below</h2>
        <div id="view">
        <div id="map" v-bind:key=pos v v-on:click="position">
          <div id="dot" v-bind:style="{left: location.x + 'px',
                      top: location.y + 'px'}">
            {{pos}} hej
          </div>
        </div>
        </div>
      </div>
    </div>
  </section>

  <button v-on:click="addOrder(key)">
    <img src="https://smallimg.pngkey.com/png/small/0-9011_simbolo-de-check-verde.png" style="height:17px; width:20px">
    Send info
  </button>


  <footer>
    <hr>
    <p> &copy; 2018 Hypothetical Burgers Inc. </p>
  </footer>

</template>

<script>
import menu from './menu.json'
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'

const socket = io();

//function MenuItem(name, url, kCal, gluten, lactose) {
 // this.name = name;
 // this.img = url;
 // this.kCal = kCal;
 // this.gluten = gluten;
  //this.lactose = lactose;
//}
//console.log(menu)
//const burgers = []
//for(var x in menu){
  //console.log(menu[x])
  //for(var y in menu[x]){
    //console.log(menu[x][y])
    //burgers[y]=menu[x][y]
  //}
//console.log(burgers[1].kCal)
//}
//const burger1 = burgers[0]
//const burger2 = burgers[1]
//const burger3 = burgers[2]
//const burger1 = new MenuItem('The fire burger','https://media-cdn.tripadvisor.com/media/photo-s/17/48/be/44/a-juicy-burger.jpg', '300', true, true)
//const burger2 = new MenuItem('Halloumi burger','https://assets.icanet.se/e_sharpen:80,q_auto,dpr_1.25,w_718,h_718,c_lfill/imagevaultfiles/id_161415/cf_259/halloumiburgare_med_srirachamajonnas_och_soltorkade_tomater.jpg', '350', true, true)
//const burger3 = new MenuItem('Vegan burger','https://receptfavoriter.se/sites/default/files/veganburgare_1060.jpg', '500', true, false)
//const burger1 = menu[Object.keys(menu)[0]]
//const burger2 = menu[Object.keys(menu)[1]]
//const burger3 = menu[Object.keys(menu)[2]]
//console.log(burger1)
//const burgermenu = [burger1, burger2, burger3]


export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers:menu,
      gender:'Undisclosed',
      payment: 'Swish',
      nr: '',
      st: '',
      em: '',
      fn:'',
      pos: 'T',
      location: Object,
      orderedBurger: Object
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    getInfo: function () {
      var form = [this.fn, this.em, this.st, this.nr, this.payment, this.gender];
      console.log(form)
      return form;
    },
    addOrder: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };
      socket.emit("addOrder", {
            orderId: this.getOrderNumber(),
            details: {
              x: event.clientX - 10 - offset.x,
              y: event.clientY - 10 - offset.y
            },
            orderItems: ["Beans", "Curry"],
            form: this.getInfo(),
          }
      );
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;

    },
    position: function (event) {
      console.log("halloj")


      var position = {x: event.clientX,
                      y: event.clientY}
      console.log(position)
      return position;
      }
    }


}
</script>

<style>
body{
  font-family: AppleGothic;
  font-size: 2.1vh;
  background: #403d41;
}

#information{
  border: 2px dotted white;
  background-image: linear-gradient(black, #950f0c);
  color: white;
}
button:hover{
  background: darkgray;
  cursor: pointer;
}
button{
  margin: 20px;
}
section{
  margin-left: 5px;
  margin-right: 5px;
  border-radius: 30px;
}
#header{
  background: white;
  overflow: hidden;
  border-radius: 30px;
  width: 100%;
  height: 30em;
}
#cover{
  opacity: 0.6;
  width: 100%;
  height: 30em;
  border-radius: 30px;
}
#welcome{
  position: absolute;
  margin-left: 20%;
  margin-top: -18%;
  font-size: 3em;
}
#custinfo{
  padding-left: 20px;
}

.choose{
  text-align: left;
  margin-left: 50px;
  font-size: large;
}
#location{
  text-align: center;
}
#infoMap{
  display: grid;
  grid-template-columns: 50% 50%;
  grid-template-rows: 100%;
}
  #view {
    height: 500px;
    width: 500px;
    color: black;
    overflow: scroll;
    margin-left: 14%;
  }

  .wrapper{
    display: grid;
    grid-template-columns: 33% 33% 33%;
    grid-template-rows: 100%;
    background: black;
    color: white;
    border: 2px dotted white;
    border-radius: 30px;
  }
  #map{
    background: url("/img/polacks.jpg");
    height: 1078px;
    width: 1920px;
  }
#dot {
  position: absolute;
  color: black;
  width:30px;
  height:30px;
  font-weight: bold;

}

</style>
