<template>
  <div id="app">
    <ul class="dice noselect">
      <li 
        v-for="c in dice" 
        :key="c.face" 
        @click.exact="countup(c.face)" 
        @click.shift="countdown(c.face)" 
        @dblclick="countdown(c.face)"
        class="face"
      >
        <div class="die" :class="diceclass(c.face)"></div>
        <div class="count">{{ c.count }}</div>
        <div class="percent" :class="percent_class(c.count)"> {{ percent(c.count) }}% </div>
      </li>
    </ul>
    <div class="others">
      <div class="total">Total: {{ total }}</div>

      <button class="reset-button" id="reset-button" @click="reset">RESET</button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: { },
  data: function() {
    return {
      dice: [
        {face: 1, count: 0},
        {face: 2, count: 0},
        {face: 3, count: 0},
        {face: 4, count: 0},
        {face: 5, count: 0},
        {face: 6, count: 0},
      ]
    }
  },
  methods: {
    countup: function(face) {
      let tmp = this.dice[face - 1]
      tmp.count += 1
      this.dice[face - 1] = tmp
    },
    countdown: function(face) {
      let tmp = this.dice[face - 1]
      tmp.count -= 1
      if (tmp.count < 0) {
        tmp.count = 0
      }
      this.dice[face - 1] = tmp
    },
    reset: function() {
      this.dice.map( (v) => {
        v.count = 0
      })
    },
    diceclass: function(face) {
      const st = ['', 'one', 'two', 'three', 'four', 'five', 'six']
      return st[face]
    },
    percent: function(cnt) {
      if (cnt === 0) return 0;
      return Math.round(1000 * cnt / this.total) / 10
    },
    percent_class: function(cnt) {
      if (this.total < 6) return;

      const threshold = 100 / 6;
      const percent = this.percent(cnt);
      let pclass = 'normal';
      if (percent > threshold + 3) {
        // high: red
        pclass = 'high';
      } else if (percent < threshold - 3) {
        // low: red
        pclass = 'low';
      } else if (percent > threshold + 1.5 ) {
        pclass = 'mid';
      } else if (percent < threshold - 1.5 ) {
        pclass = 'mid';
      } else {
        // normal
        pclass = 'normal';
      }
      return pclass
    },

    keybindings: function(event) {
      const keylist = ["1", "2", "3", "4", "5", "6"];
      if (keylist.indexOf(event.key) < 0) return

      const face = event.key / 1; // to int
      this.countup(face)
    },
  },
  computed: {
    total: function() {
      var total_n = 0
      this.dice.map((v) => {
        total_n += v.count
      })
      return total_n
    },
  },
  created() {
    window.addEventListener("keydown", this.keybindings);
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

ul.dice {
  display: flex;
  flex-direction: row;
  list-style: none;
  align-items: center;
  justify-content: center;
}

li.face {
  width: 100px;
  margin: 5px;
  padding: 15px;
  border: 1px solid #ccc;
  border-radius: 10px;
  background-color: #fff;

}

li.face:hover {
  background-color: #f9f9f9; 
}

li.face:active {
  background-color: #eee;
}

div.count {
  font-size: 3em;
  font-weight: bold;
  font-family: monospace;
  color: #666
}

div.percent,
div.percent.normal {
  color: #9c9;
}

div.percent.high {
  color: #f99;
}

div.percent.mid {
  color: #fc9;
}

div.percent.low {
  color: #9af;
}

/* DICE */
.die.one:after { 
  box-shadow: 0 .2em 0 #FFF;
  background: #fa5444;
}

.die.two:after { 
  background: transparent;
  box-shadow: -2.3em -2.3em 0 #345, 2.3em  2.3em 0 #345, -2.3em -2.3em 0 #FFF, 2.3em  2.4em 0 #FFF;
}

.die.three:after { 
  box-shadow: -2.3em -2.3em 0 #345, 2.3em  2.3em 0 #345, -2.3em -2.3em 0 #FFF, 2.3em  2.4em 0 #FFF, 0 .2em 0 #FFF;
}

.die.four:after { 
  background: transparent;
  box-shadow: -2.3em -2.3em 0 #345, 2.3em  2.3em 0 #345, -2.3em  2.3em 0 #345, 2.3em -2.3em 0 #345, -2.3em -2.3em 0 #FFF, 2.3em  2.4em 0 #FFF, -2.3em  2.4em 0 #FFF, 2.3em -2.3em 0 #FFF;
}

.die.five:after {
  box-shadow: -2.3em -2.3em 0 #345, 2.3em  2.3em 0 #345, -2.3em  2.3em 0 #345, 2.3em -2.3em 0 #345, -2.3em -2.2em 0 #FFF, 2.3em -2.2em 0 #FFF, 2.3em  2.4em 0 #FFF, -2.3em  2.4em 0 #FFF, 0 .2em 0 #FFF;
}

.die.six:after {
  background: transparent;
  box-shadow: -2.3em -2.3em 0 #345, -2.3em 0 0 #345, -2.3em  2.3em 0 #345, 2.3em -2.3em 0 #345, 2.3em      0 0 #345, 2.3em  2.3em 0 #345, -2.3em -2.1em 0 #FFF, -2.3em   .2em 0 #FFF, -2.3em  2.4em 0 #FFF, 2.3em  2.4em 0 #FFF, 2.3em   .2em 0 #FFF, 2.3em -2.1em 0 #FFF;
}

.die {
  border-top: 1px solid #f1f1f1;
  border: 1px solid #eee;
  width: 50px; height: 50px;
  border-radius: 10px;
  position: relative;
  margin: 0 10px 20px 10px;
  font-size: 6px;
  display: inline-block;
  box-shadow: 0px 5px 0 #CCC, 0 6px 3px #444, 0 10px 5px #999;
  background-image: linear-gradient(to top, #fefefe, #f1f1f1 80%)
}

.die:after {
  content: "";
  width: 20%; 
  height: 20%; 
  left: 50%; 
  top: 50%;
  margin: -10%;
  background: #345;
  border-radius: 50%;
  display: block;
  position: absolute;
}

.others {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}

.others > * {
  margin: 10px;
}

.noselect {
  -webkit-touch-callout: none; /* iOS Safari */
    -webkit-user-select: none; /* Safari */
     -khtml-user-select: none; /* Konqueror HTML */
       -moz-user-select: none; /* Old versions of Firefox */
        -ms-user-select: none; /* Internet Explorer/Edge */
            user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}
</style>
