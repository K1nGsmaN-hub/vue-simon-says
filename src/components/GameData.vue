<template>
  <div class="game-data">
    <div class="round-counter">
      <span>Round: </span>
      <span>{{ round }}</span>
    </div>

    <form @submit.prevent>
      <button v-on:click="$emit('game-start', diffLvl)">Start</button>

      <div class="lose-msg">
        <p>{{ loseMsg }}</p>
      </div>

      <div class="game-options">
        <h1>
          Difficulty level:
        </h1>
        <div class="radio-buttons">
          <label>
            <input type="radio" name="lvl-diff" value="1500" checked
                   v-model="diffLvl"/> Easy
          </label>
          <label>
            <input type="radio" name="lvl-diff" value="1000"
                   v-model="diffLvl"/> Medium
          </label>
          <label>
            <input type="radio" name="lvl-diff" value="400"
                   v-model="diffLvl"/> Hard
          </label>
        </div>
      </div>
    </form>

  </div>
</template>

<script>
  export default {
    props: ['round', 'loseMsg'],
    data() {
      return {
        diffLvl: ''
      }
    },
    methods: {
      show() {
        this.$emit('diff-lvl', this.diffLvl)
      }
    },
  }
</script>

<style lang="sass" scoped>
  .game-data
    width: 190px
    display: flex
    flex-direction: column
    align-items: flex-start
    text-align: left

    span
      font-family: Arial, sans-serif
      font-weight: bold
      font-size: 25px

    button
      width: 120px
      height: 40px
      margin-top: 20px
      border: none
      background-color: lightblue
      border-radius: 8px
      font-family: Arial, sans-serif
      font-size: 25px

    .lose-msg
      margin-top: 20px

    h1
      font-family: Arial, sans-serif
      font-size: 25px
      font-weight: bold
      margin-top: 30px

    .radio-buttons
      margin-top: 5px
      display: flex
      flex-direction: column
      align-items: flex-start
      font-family: Arial, sans-serif

      label
        margin-bottom: 5px
        margin-left: 5px

  @media screen and (max-width: 568px)
    .game-data
      text-align: center
      align-items: center
</style>
