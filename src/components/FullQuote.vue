<template>

  <div class="fullquote">
    <div id="color-overlay" ref="colorElement"> </div>
    <button @click="fadeColor(); buttonDisabled = true;"> Get Quote </button>
    <div class="quote">
      <h1>
        <span class="quote-mark">"</span>
          {{ quote.quote }}
        <span class="quote-mark">"</span>
      </h1>
      <h2 class="author">{{ quote.author }} </h2>
    </div>
  </div>

</template>

<script>
import axios from 'axios'

export default {
  name: 'fullquote',
  data () {
    return {
      quote: {},
      buttonDisabled: false,
      colors: ['#183668', '#0a6882', '#0b634c', '#40175e',
      '#5e1756', '#5e172c', '#415e17', '#5e4e17', '#5e3b17', '#5e5c17', '#560f0f']
    }
  },

  methods:{
    getQuote() {
      var self = this;
      axios.get('https://talaikis.com/api/quotes/random')
        .then(function (response) {
              self.quote = response.data;
        })
        .catch(function (error) {
            console.log(error);
        });

    },
    fadeColor() {
      if(!this.buttonDisabled){
        this.$refs.colorElement.style.opacity = 0;
      }
    },
    setNewColor(object) {
      var randomColor = this.colors[Math.floor(Math.random() * this.colors.length)];
      object.style.background = randomColor;
    }
  },
  mounted() {
      var self = this;
      this.getQuote()
      //at the end of fade out update the quote and set a new color
      this.$refs.colorElement.addEventListener('webkitTransitionEnd', function(){
        if(this.style.opacity == 0){
          self.getQuote();
          self.setNewColor(this);
        }
        this.style.opacity = 1;
        self.buttonDisabled = false;
      }, false);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

h1, h2 {
  font-weight: normal;
  color: white;
}

#color-overlay{
  position:absolute;
  top: -60px;
  z-index: -1;
  min-width:100%;
  min-height:100%;
  height: 107%;
  background-color: #183668;
}

html{
  padding:0;
}

#color-overlay{
  animation-duration: 3s;
  transition: opacity 3s;
  opacity: 1;
}

.author{
  float:right;
  margin-right: 10%;
}

.quote{
  padding-left:10%;
  padding-right:10%;
  margin-top: 10%;
}

.quote-mark{
  font-size: 70px;
}
</style>
