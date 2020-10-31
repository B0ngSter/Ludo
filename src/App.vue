<template>
<div id="app">
  <div id="main">
  <div id="board"></div>
  <!-- <img alt="Vue logo" src="./assets/dice.gif"> -->
  <div @click="dice">
    <v-btn>Button {{ number }}</v-btn>
  </div>
  <!-- <div id="dice" onclick="randomNum()" style="background-image:url(./assets/dice.gif);background-size:contain; width:150px;height:150px; float:left;"></div> -->
  <!-- Red pawns  start top 28 left 318-->
  <div class="pawns" id="Redpawn1"    @click="randomMove('Red',1)"       style="background-color:red;top:149px;left:442px;"></div>
  <div class="pawns" id="Redpawn2"    @click="randomMove('Red',2)"       style="background-color:red;top:102px;left:395px;"></div>
  <div class="pawns" id="Redpawn3"    @click="randomMove('Red',3)"       style="background-color:red;top:55px;left:442px;"></div>
  <div class="pawns" id="Redpawn4"    @click="randomMove('Red',4)"       style="background-color:red;top:102px;left:490px;"></div>
  <!-- Yellow pawns  start top 523 left 218-->
  <div class="pawns" id="Yellowpawn1" @click="randomMove('Yellow',1)"    style="background-color:Yellow;top:451px;left:47px;"></div>
  <div class="pawns" id="Yellowpawn2" @click="randomMove('Yellow',2)"    style="background-color:Yellow;top:451px;left:140px;"></div>
  <div class="pawns" id="Yellowpawn3" @click="randomMove('Yellow',3)"    style="background-color:Yellow;top:404px;left:93px;"></div>
  <div class="pawns" id="Yellowpawn4" @click="randomMove('Yellow',4)"    style="background-color:Yellow;top:498px;left:93px;"></div>
  <!-- Green pawns  start top 227 left 22-->
  <div class="pawns" id="Greenpawn1"  @click="randomMove('Green',1)"     style="background-color:Green;top:149px;left:93px;"></div>
  <div class="pawns" id="Greenpawn2"  @click="randomMove('Green',2)"     style="background-color:Green;top:102px;left:140px;"></div>
  <div class="pawns" id="Greenpawn3"  @click="randomMove('Green',3)"     style="background-color:Green;top:55px;left:93px;"></div>
  <div class="pawns" id="Greenpawn4"  @click="randomMove('Green',4)"     style="background-color:Green;top:102px;left:47px;"></div>
  <!-- Blue pawns  start top 326 left 515-->
  <div class="pawns" id="Bluepawn1"   @click="randomMove('Blue',1)"      style="background-color:Blue;top:451px;left:490px;"></div>
  <div class="pawns" id="Bluepawn2"   @click="randomMove('Blue',2)"      style="background-color:Blue;top:451px;left:395px;"></div>
  <div class="pawns" id="Bluepawn3"   @click="randomMove('Blue',3)"      style="background-color:Blue;top:404px;left:442px;"></div>
  <div class="pawns" id="Bluepawn4"   @click="randomMove('Blue',4)"      style="background-color:Blue;top:498px;left:442px;"></div>
<div>
  <span>Its's </span>
  <span :class="{'red' : player === 'Red', 'yellow' : player === 'Yellow', 'blue' : player === 'Blue', 'green' : player === 'Green',}">{{ player }}</span>
  <span> turn</span><span> {{ checked }}</span>
</div>
<div style="width: 100vw;">
  <ul v-for="(digit, player, idx) in positions" :key="idx">
    <li>
      {{ player }} : {{ digit }}
    </li>
  </ul>
</div>
<div>
  <ul v-for="(digit, player, idx) in card_map" :key="idx">
    <li>
      {{ player }} : {{ digit }}
    </li>
  </ul>
</div>
<div>
  <ul v-for="(digit, player, idx) in goNext" :key="idx">
    <li>
      {{ player }} : {{ digit }}
    </li>
  </ul>
</div>
<div style="width: 100vw;">
  <ul v-for="(player, idx) in trapped" :key="idx">
    <li>
      {{ player }}
    </li>
  </ul>
</div>
<!-- <h4 id="uselesstext1">It`s </h4><h3 id="player" style="float:left; color:Red">Red</h3><h4 id="uselesstext2">s turn</h4> -->
<p id="badtext" style="float:left;"></p>
</div>
</div>
</template>

<script>
// import Vue from 'vue'
export default {
  name: 'App',
  data () {
    return {
      number: null,
      player: 'Red',
      checked: true, //clear to go to next
      positions: {
          Redpawn1: 0, Redpawn2: 0, Redpawn3: 0, Redpawn4: 0,
          Bluepawn1: 0, Bluepawn2: 0, Bluepawn3: 0, Bluepawn4: 0,
          Greenpawn1: 0, Greenpawn2: 0, Greenpawn3: 0, Greenpawn4: 0,
          Yellowpawn1: 0, Yellowpawn2: 0, Yellowpawn3: 0, Yellowpawn4: 0
      },
      card_map: {
          Greenpawn1: 0, Greenpawn2: 0, Greenpawn3: 0, Greenpawn4: 0,
          Redpawn1: 10, Redpawn2: 20, Redpawn3: 10, Redpawn4: 10,
          Bluepawn1: 21, Bluepawn2: 22, Bluepawn3: 23, Bluepawn4: 24,
          Yellowpawn1: 30, Yellowpawn2: 30, Yellowpawn3: 30, Yellowpawn4: 30
      },
      trapped: {
        Red: ['Redpawn1', 'Redpawn2', 'Redpawn3', 'Redpawn4'],
        Yellow: ['Yellowpawn1', 'Yellowpawn2', 'Yellowpawn3', 'Yellowpawn4'],
        Blue: ['Bluepawn1', 'Bluepawn2', 'Bluepawn3', 'Bluepawn4'],
        Green: ['Greenpawn1', 'Greenpawn2', 'Greenpawn3', 'Greenpawn4']
      },
      goNext: {},
      rollAgain: false
    }
  },
  methods: {
    dice () {
      this.checked = false
      if (!this.rollAgain) {
        // this.number = 6
        this.number = Math.floor((Math.random() * 6) + 1)
      }
      if (this.trapped[this.player].length === 4 && this.number != 6) { // player switch krne k liye
        this.switchPlayer()
      } else if (this.trapped[this.player].length != 4) { // dubara roll na ho dice
        this.rollAgain = true
      }
    },
    randomMove (color, num) {
      const el = color + 'pawn' + num
      // const element = document.getElementById(el)
      if (this.number === 6 && this.trapped[color].includes(el) && !this.checked) { // bahar nikalne k liye
        this.trapped[color].splice(this.trapped[color].indexOf(el), 1)
        this.checked = true
      } else if (this.number && !this.trapped[color].includes(el) && !this.checked) { // chaal chalne k liye
        this.positions[el] += this.number
        this.card_map[el] += this.number
        this.rollAgain = false
        this.checked = true
        /* eslint-disable no-debugger */
          debugger
        /* eslint-enable no-debugger */
        this.switchPlayer()
        if (this.card_map[el] > 39) {
          this.card_map[el] -= 39
        }
        // const dummyObj = {}
        for ( var i = 0; i < Object.keys(this.card_map).length; i++ ) {
          // const ab = Object.keys(this.card_map)[i].slice(0, Object.keys(this.card_map)[i].length - 2)
          // Vue.set(dummyObj, ab, this.card_map[Object.keys(this.card_map)[i]])
          // const arr = [...new Set(Object.keys(dummyObj))]
          for ( var j = 1; j < Object.keys(this.card_map).length; j++ ) {
            if (this.card_map[Object.keys(this.card_map)[i]] === this.card_map[Object.keys(this.card_map)[j]] && Object.keys(this.card_map)[i].slice(0, Object.keys(this.card_map)[i].length - 2) != Object.keys(this.card_map)[j].slice(0, Object.keys(this.card_map)[j].length - 2)) {
              this.positions[el] = 0
              switch (color) {
              case "Red":
                this.card_map[el] = 10
                this.player = "Blue"
                break
              case "Yellow":
                this.card_map[el] = 30
                this.player = "Green"
                break              
              case "Blue":
                this.card_map[el] = 20
                this.player = "Yellow"
                break
              case "Green":
                this.card_map[el] = 0
                this.player = "Red"
                break
              }
              this.trapped[color].push(el)
            }
          }
        }
        // this.goNext = dummyObj
      }
    },
    switchPlayer () {
      switch (this.player) {
      case "Red":
        this.player = "Blue"
        break
      case "Yellow":
        this.player = "Green"
        break              
      case "Blue":
        this.player = "Yellow"
        break
      case "Green":
        this.player = "Red"
        break
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#main{
  width:1000px;
  float:left;
}
.pawns{
  width:30px;
  height:30px;
  margin-left: -8px;
  margin-top: 52px; 
  border-radius:20px;
  position:absolute;
}
#board{
  background:url(./assets/board.gif) no-repeat;
  background-size:cover;
  width:650px;
  height:550px;
  float:left;
  margin-top:8px;
}
#uselesstext1{
  float:left;
  padding-left:10px;
  padding-right:10px;
}
footer{
  font-size:larger;
  color:darkred;
}
.red {
  color: red;
}
.blue {
  color: blue;
}
.yellow {
  color: yellow;
}
.green {
  color: green;
}
</style>
