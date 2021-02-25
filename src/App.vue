<template>
  <div id="app">
    <div id="header">
      <div>
        rgb({{red}}, {{green}}, {{blue}})
      </div>
      <div id="score">
        score: {{score}}
      </div>
      <div id="time">
        time: {{time}}
      </div>
    </div>

    <div v-if="score == 0 || time == 0" class="pop-up">
      <div class="scrim"></div>
      <div v-if="score == 0" class="content">
        you lose!
      </div>
      <div v-else class="content">
        <p>
          you're out of time!
        </p>
        <p>
          your score was: {{score}}.
        </p>
      </div>
    </div>

    <div id="grid">
      <div
      v-for="color in colors"
      :key="color"
      :style="{'background-color':color}"
      @click="checkColor(color)"
      >
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: "App",
  data: function() {
    return {
      red: Math.floor((Math.random() * 256)),
      green: Math.floor((Math.random() * 256)),
      blue: Math.floor((Math.random() * 256)),
      score: 4,
      time: 3,
      frame: 0
    }
  },
  computed: {
    colors: function () {
      let nonRandom = [`rgb(${this.red},${this.green},${this.blue})`]
      for (let i = 0; i < 3; i++) {
        nonRandom.push(`rgb(${Math.floor((Math.random() * 256))},${Math.floor((Math.random() * 256))},${Math.floor((Math.random() * 256))})`)
      }
      let random = []
      for (let i = 0; i < 4; i++) {
        let randColor = nonRandom.splice(Math.floor((Math.random() * nonRandom.length) ), 1)
        random.push(randColor[0])
      }
      return random
    }
  },
  methods: {
    checkColor: function (color) {
      let correctColor = `rgb(${this.red},${this.green},${this.blue})`
      if (color == correctColor) {
        this.score += 1
        this.red = Math.floor((Math.random() * 256))
        this.green = Math.floor((Math.random() * 256))
        this.blue = Math.floor((Math.random() * 256))
      }
      // if color != correctColor, lose one point
      if (color != correctColor) {
        this.score -= 1
        this.red = Math.floor((Math.random() * 256))
        this.green = Math.floor((Math.random() * 256))
        this.blue = Math.floor((Math.random() * 256))
      }
    },
    countDown: function() {
      this.frame += 1
      if (this.frame == 60) {
        this.frame = 0
        this.time -= 1 // change to 1 once you fix
      }
      if (this.time != 0) {
        requestAnimationFrame(this.countDown)
      }

    }
  },
  mounted: function() {
    requestAnimationFrame(this.countDown)
  }
};
</script>

<style>
html,body {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
}

#app {
  width: 100%;
  height: 100%;
}

#header {
  box-sizing: border-box;
  width: 100%;
  height: 100px;
  padding: 20px 0 20px 0; /* top right bottom left */
  text-align: center;
  line-height: 60px;
  font-size: 28px;
}

#score {
  position: absolute;
  top: 0;
  right: 5px;
  line-height: 28px;
  font-size: 28px;
}

#time {
  position: absolute;
  top: 0;
  left: 5px;
  line-height: 28px;
  font-size: 28px;
}

.pop-up {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 3000;
}

.scrim {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background-color: rgba(0,0,0,0.75);
}

.content {
  position: absolute;
  z-index: 7;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-size: 56px;
  color: white
}

#grid {
  display: grid;
  width: 100%;
  height: calc(100% - 100px);
  grid-template-columns: 50% 50%;
  grid-template-rows: 50% 50%;
}
</style>
