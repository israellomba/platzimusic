<template lang="pug">
  #app
    img(src='dist/logo.png')
    h1 Platzimusic
    select(v-model="selectedCountry")
      option(v-for="country in countries" v.bind:value="country.value") {{country.name}}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")

</template>

<script>
import getArtists from './api'
import  Artist from './components/Artist.vue'
import spinner from './components/spinner.vue'
export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        {name: 'Argentina', value:'argentina'},
        {name: 'Colombia', value:'colombia'},
        {name: 'Mexico', value:'mexico'},
      ],
      selectedCountry: 'argentina',
      loading:true
    }
  },
  components: {
    Artist,
    spinner
  },
  methods: {
    refreshArtist() {
      const self = this
      this.loading = true
      this.artists = []
      getArtists(this.selectedCountry)
        .then(function (artists) {
          self.artists = artists,
          self.loading = false
        })
    }
  },
  mounted() {
    this.refreshArtist()
  },
  watch: {
    selectedCountry: function (){
      this.refreshArtist()
    }
  }
}
</script>

<style lang="stylus">
#app 
	font-family 'Avenir', Helvetica, Arial, sans-serif
	-webkit-font-smoothing antialiased
	-moz-osx-font-smoothing grayscale
	text-align center
	color #2c3e50
	margin-top 60px
h1, h2 
	font-weight normal
ul 
  display block
	list-style-type none
  margin-block-start 1em
  margin-block-end 1em
  margin-inline-start 0px
  margin-inline-end 0px
  padding-inline-start 40px
	padding 0
li 
  display inline-block
  padding 5px
  margin 10px
  border 2px solid #41b883
  border-radius 5px
	margin 0 10px
a 
	color #42b983
</style>
