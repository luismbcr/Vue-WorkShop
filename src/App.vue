

<template lang="pug">
#app
  h1 Top Artists List 
  select(v-model="selectedCountry")
    option(v-for="country in countries" :value="country.value") {{country.name}}
  spinner(v-show="loading")
  ul(class="music-list")
    artist(v-for="artist in  artists" v-bind:artist="artist" :key="artist.mbid")
</template>

<script>
import Artist from "./components/Artist.vue";
import Spinner from "./components/Spinner.vue";
import getArtists from "./api";
export default {
  name: "app",
  data() {
    return {
      artists: [],
      countries: [
        { name: "Argentina", value: "argentina" },
        { name: "España", value: "spain" },
        { name: "Costa Rica", value: "costa rica" }
      ],
      selectedCountry: "argentina",
      loading: true
    };
  },
  components: {
    Artist,
    Spinner
  },
  methods: {
    refreshList(country) {
      const self = this
      this.artists= []
      this.loading= true
      getArtists(country).then(function(artists) {
        self.loading=false
        self.artists = artists;
      });
    }
  },
  mounted: function() {
    this.refreshList(this.selectedCountry)
  },
  watch: {
    selectedCountry: function() {
      this.refreshList(this.selectedCountry)
    }
  }
};
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px
a
  color #42b983
.music-list
  display: flex
  flex-wrap: wrap
  justify-content: center
</style>
