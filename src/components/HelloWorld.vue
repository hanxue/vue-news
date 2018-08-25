<template>
  <v-container fluid grid-list-md>
    <v-slide-y-transition mode="out-in">
      <v-layout column align-center>
        <img src="@/assets/logo.png" alt="Vue top stories" class="mb-5">
        <h3 class="text-center">Vue Top Stories</h3>
        <v-data-iterator
          :items="results"
          content-tag="v-layout"
          row
          wrap
        >
        <v-flex
        slot="item"
        slot-scope="props"
        xs12
        sm6
        md4
        lg3
      >
        <v-card :href="props.item.url">
          <v-card-media
          class="white--text"
          height="200px"
          :src="props.item.multimedia[2].url"
        >
          <v-container fill-height fluid>
            <v-layout fill-height>
              <v-flex xs12 align-end flexbox>
                <span class="headline">{{ props.item.title }}</span>
              </v-flex>
            </v-layout>
          </v-container>
        </v-card-media>
          <v-card-title>
            <div class="headline">{{ props.item.des_facet[0] }}</div>
            <div>{{ props.item.abstract }}</div>
          </v-card-title>
        </v-card>
        </v-flex>
        </v-data-iterator>
      </v-layout>
    </v-slide-y-transition>
  </v-container>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
 
Vue.use(VueAxios, axios)

const NYTBaseUrl = "https://api.nytimes.com/svc/topstories/v2/home.json?api-key="
// Get your API key from https://developer.nytimes.com/signup
const ApiKey = "xxxxxxxxxxxxxxxxx"

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data: function() {
		return {
      results: [],
    }
  },
  mounted: function() {
    Vue.axios.get(NYTBaseUrl + ApiKey)
    .then(response => { this.results = response.data.results })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
