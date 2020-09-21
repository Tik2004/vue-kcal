<template>
  <div id="app">
    <h1 id="welcome-text">Get food Calories</h1>
    <h2 id="by-author">By Tigran Arshakyan</h2>
    <h2 class="github-link">
      <a href="https://github.com/Tik2004" target="_blank">My Other Projects</a>
    </h2>
    <div class="form">
      <input
        v-model="foodName"
        @keypress.enter="search"
        id="name-of-product"
        type="text"
        placeholder="English name of product"
      />
      <button @click="search" class="search-btn">Search</button>
    </div>
    <div v-if="result.foods">
      <div class="results-wrapper" v-for="(food, i) in result.foods" :key="i">
        <div
          :style="{
            'background-color': colors[Math.floor(Math.random() * 12)]
          }"
          class="result-box"
        >
          <h1>
            {{
              food.description ? food.description : food.additionalDescription
            }}
          </h1>
          <h1>
            {{
              food.foodNutrients.filter(m => m.nutrientName === "Energy")[0]
                .value
            }}
            KCal on 100gr
          </h1>
          <h2 id="choice">
            {{
              getChoice(
                food.foodNutrients.filter(m => m.nutrientName === "Energy")[0]
                  .value
              )
            }}
          </h2>

          <h2>{{ food.ingredients }}</h2>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      ApiKey: "jGGbefeWyloOn9P1TxEglM5X3Ohy8HUWf7gyUXJ9",
      baseUrl: "https://api.nal.usda.gov/fdc/v1/foods/search/?query=",
      afterOver: "&api_key=",
      foodName: "",
      ingredients: "",
      calories: "",
      showName: "",
      result: "",
      goodChoices: [
        "Good choice",
        "Nice to eat",
        "Best for diet",
        "You can eat it, and stay calm",
        "good for your body",
        "Eat it if you are on diet",
        "Not bad choice"
      ],
      badChoices: [
        "Not suggesting you to eat that",
        "Not good choice for diet",
        "Better do not eat that on diet",
        "Not best choice for diet",
        "Actaually, do not eat that",
        "It may be yummy, but you will get fat",
        "This food can help you to gain bigger belly"
      ],
      colors: [
        "#e91e63",
        "#e53935",
        "#d500f9",
        "#1e88e5",
        "#283593",
        "#512da8",
        "#039be5",
        "#00acc1",
        "#7cb342",
        "#f9a825",
        "#ff6f00",
        "#2e7d32"
      ]
    };
  },
  methods: {
    async search() {
      const result = await fetch(
        this.baseUrl + this.foodName + this.afterOver + this.ApiKey // Get data about searched query food
      );
      this.result = await result.json(); // parse data
    },
    getChoice(value) {
      if (value <= 250) {
        return this.goodChoices[Math.floor(Math.random() * 7)];
      } else {
        return this.badChoices[Math.floor(Math.random() * 7)];
      }
    }
  }
};
</script>

<style lang="stylus">
@import url('https://fonts.googleapis.com/css2?family=Open+Sans&display=swap')
@import url('https://fonts.googleapis.com/css2?family=Ranchers&display=swap');

#app
  font-family 'Open Sans'
  font-weight 600
  text-align center
  color rgb(49,48,48)
  padding 30px

#welcome-text
  font-size 3.5rem

#by-author
  font-size 2rem

#name-of-product
  padding .5rem
  border-radius 5px
  border 1.5px solid #0288D1
  width 14rem
  font-size 2rem
  transition-duration 0.2s
  &:focus
    box-shadow 0 2px 14px #0288D1

.results-wrapper
  display inline-flex
  margin 10px


.result-box
  padding 1rem
  border-radius 20px
  color white
  box-shadow 0 0 20px gray, 0 0 22px gray, 0 0 5px gray 


.search-btn
  width 10rem
  display block
  margin 1rem auto
  background #43a047
  padding .8rem
  font-size 1.3rem
  color #fff
  border 1px solid #aed581
  border-radius 10px
  transition-duration 0.2s
  &:hover
    box-shadow 0 0 20px #aed581, 0 0 10px #aed581, 0 0 10px #aed581, 0 0 28px #aed581

#choice
  font-family: 'Ranchers', cursive;
  font-weight 100
  font-size 2rem
  color rgba(255,255,255,0.7)

.github-link
  color #e90f63
  font-size 1.6rem
  margin 1rem
  transition-duration 0.3s
  &:hover,&:after,&:focus
    text-shadow 0 0 4px #e90f63
    color #e90f63
*
  outline none
  text-decoration none
</style>
