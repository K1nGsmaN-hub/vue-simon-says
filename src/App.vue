<template>
  <div id="app">
    <ul class="game-area" @click="getElemActive">
      <li class="blue top-left"
          data-id="1"
          v-bind:class="{active: buttons.blue.isActive}"></li>
      <li class="red top-right"
          data-id="2"
          v-bind:class="{active: buttons.red.isActive}"></li>
      <li class="yellow bottom-left"
          data-id="3"
          v-bind:class="{active: buttons.yellow.isActive}"></li>
      <li class="green bottom-right"
          data-id="4"
          v-bind:class="{active: buttons.green.isActive}"></li>
    </ul>
    <GameData v-on:game-start="startGame"
              v-bind:round="round"
              v-bind:loseMsg="loseMsg"/>
  </div>
</template>

<script>
  import GameData from '@/components/GameData'

  export default {
    name: 'App',
    data() {
      return {
        gameSequence: [],
        userSequence: [],
        buttons: {
          blue: {id: 1, color: 'blue', isActive: false, sound: new Audio(require('./assets/sounds/1.mp3'))},
          red: {id: 2, color: 'red', isActive: false, sound: new Audio(require('./assets/sounds/2.mp3'))},
          yellow: {id: 3, color: 'yellow', isActive: false, sound: new Audio(require('./assets/sounds/3.mp3'))},
          green: {id: 4, color: 'green', isActive: false, sound: new Audio(require('./assets/sounds/4.mp3'))},
        },
        round: 0,
        loseMsg: '',
        delay: 0
      }
    },
    components: {
      GameData
    },
    methods: {
      generateNewStep() {
        let newStep = 0
        while (true) {
          newStep = this.randomInt()
          if (newStep === this.gameSequence[this.gameSequence.length - 1]) {
            continue
          } else {
            break
          }
        }
        return newStep
      },
      startGame(data) {
        this.delay = Number(data)
        if (this.delay > 0) {
          console.log('Game start...')
          console.log(`Your delay = ${this.delay}`)
          this.loseMsg = ''
          this.round = 1
          this.userSequence = []
          this.gameSequence = [this.randomInt()]
          this.showSequence(this.delay)
          this.gameSession()
        } else {
          console.log('Please choose difficulty level!')
        }
      },
      gameSession() {
        let intervalId = setInterval(() => {
          if (this.gameSequence.length === this.userSequence.length) {

            let correctValues = this.gameSequence.length

            this.gameSequence.forEach((value, index) => {
              if (value !== this.userSequence[index]) {
                correctValues -= 1
              }
            })
            if (correctValues === this.gameSequence.length) {
              console.log('win')
              this.round += 1
              this.userSequence = []
              this.gameSequence.push(this.generateNewStep())
              this.showSequence(this.delay)
              return this.gameSession()
            } else {
              clearInterval(intervalId)
              this.loseMsg = `You lose :( Don't worry! Try again!`
              this.round = 0
              return console.log('lose')
            }
          } else if (this.gameSequence.length < this.userSequence.length) {
            clearInterval(intervalId)
            this.loseMsg = `You lose :( Don't worry! Try again!`
            console.log(this.round)
            this.round = 0
            return console.log('lose')
          } else if (this.gameSequence.length > this.userSequence.length) {

            let correctValue = this.userSequence.length

            this.userSequence.forEach((value, index) => {
              if (value !== this.gameSequence[index]) {
                correctValue -= 1
              }
              if (correctValue !== this.userSequence.length) {
                clearInterval(intervalId)
                this.loseMsg = `You lose :( Don't worry! Try again!`
                console.log(this.round)
                this.round = 0
                return console.log('lose')
              }
            })
          }
        }, 1000)
      },
      randomInt() {
        return Math.round(0.5 + Math.random() * 4)
      },
      showSequence(delay) {
        let i = 0
        this.gameSequence.forEach((val) => {
          i += 1
          setTimeout(() => {
            let j = 1
            for (let item in this.buttons) {
              j += 0.001
              if (this.buttons[item].id === val) {
                this.buttons[item].isActive = true
                this.buttons[item].sound.play()
                setTimeout(() => {
                  this.buttons[item].isActive = false
                }, j * 200)
              }
            }
          }, i * delay)
        })
      },
      getElemActive(e) {
        switch (Number(e.target.dataset.id)) {
          case (1):
            this.buttons.blue.isActive = true
            this.buttons.blue.sound.play()
            setTimeout(() => {
              this.buttons.blue.isActive = false
            }, 200)
            break
          case (2):
            this.buttons.red.isActive = true
            this.buttons.red.sound.play()
            setTimeout(() => {
              this.buttons.red.isActive = false
            }, 200)
            break
          case (3):
            this.buttons.yellow.isActive = true
            this.buttons.yellow.sound.play()
            setTimeout(() => {
              this.buttons.yellow.isActive = false
            }, 200)
            break
          case (4):
            this.buttons.green.isActive = true
            this.buttons.green.sound.play()
            setTimeout(() => {
              this.buttons.green.isActive = false
            }, 200)
            break
        }

        this.userSequence.push(Number(e.target.dataset.id))
      },
    }
  }
</script>

<style lang="sass">
  @import "public/main.sass"

  #app
    margin-top: 80px
    display: flex
    justify-content: space-between
    align-items: center
    min-height: 300px

  @media screen and (max-width: 568px)
    #app
      margin-top: 40px
      flex-direction: column

    .game-area
      margin-bottom: 40px

  .game-area
    width: 280px
    height: 280px
    border: 4px solid #fff
    border-radius: 100%
    box-shadow: 0 0 10px rgba(#000, 0.25)

    display: grid
    grid-template-columns: repeat(2, 1fr)
    grid-template-rows: repeat(2, 1fr)

    li
      cursor: pointer


  .top-left
    border-top-left-radius: 100%

    &:hover
      border-top: 2px solid #000
      border-left: 2px solid #000

  .top-right
    border-top-right-radius: 100%

    &:hover
      border-top: 2px solid #000
      border-right: 2px solid #000

  .bottom-left
    border-bottom-left-radius: 100%

    &:hover
      border-left: 2px solid #000
      border-bottom: 2px solid #000

  .bottom-right
    border-bottom-right-radius: 100%

    &:hover
      border-bottom: 2px solid #000
      border-right: 2px solid #000

  .blue
    background-color: rgba(blue, 0.5)

    &.active
      background-color: blue

  .red
    background-color: rgba(red, 0.5)

    &.active
      background-color: red

  .yellow
    background-color: rgba(yellow, 0.5)

    &.active
      background-color: yellow

  .green
    background-color: rgba(green, 0.5)

    &.active
      background-color: green
</style>
