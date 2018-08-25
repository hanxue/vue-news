<template>
  <v-container fluid grid-list-md>
    <v-slide-y-transition mode="out-in">
      <v-layout column align-center>
        <img src="@/assets/logo.png" alt="Vue top stories" class="mb-5">
        <v-flex xs12>
        <v-form full-width>
          <v-text-field
            v-model="apiKey"
            @change="fetchNews"
            label="New York Times API Key"
            required
          ></v-text-field>
          <v-autocomplete
            v-model="selectedCategories"
            :items="categories"
            :disabled="isEditing"
            box
            chips
            label="Filter news Category"
            item-text="category"
            item-value="category"
            multiple
          >
            <template
              slot="selection"
              slot-scope="data"
            >
              <v-chip
                :selected="data.selected"
                close
                class="chip--select-multi"
                @input="data.parent.selectItem(data.item)"
              >
                {{ data.item }}
              </v-chip>
            </template>
            <template
              slot="item"
              slot-scope="data"
            >
              <template>
                <v-list-tile-content v-text="data.item"></v-list-tile-content>
              </template>
            </template>
          </v-autocomplete>
        </v-form>
        </v-flex>
        <h3 class="text-center">Vue Top Stories</h3>
        <v-data-iterator
          :items="filteredResults"
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
    <v-alert
      v-model="alert"
      dismissible
      type="warning"
    >
      {{ alertMessage }}
    </v-alert>
  </v-container>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
 
Vue.use(VueAxios, axios)

const NYTBaseUrl = "https://api.nytimes.com/svc/topstories/v2/home.json?api-key="

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data: function() {
		return {
      results: [],
      apiKey: 'xxxxxx',
      alert: 'true',
      alertMessage: 'Enter your New York Times API Key from https://developer.nytimes.com/signup',
      isEditing: false,
      allCategories: new Set(),
      selectedCategories: [],
      categories: [],
    }
  },
  mounted: function() {
    this.fetchNews()
  },
  watch: {
    isEditing (val) {
      if (val) {
        setTimeout(() => (this.isUpdating = false), 3000)
      }
    }
  },
  methods: {
    fetchNews() {
      Vue.axios.get(NYTBaseUrl + this.apiKey)
        .then(response => { 
          this.alert = false
          this.results = response.data.results
          this.results.map( result => {
            return result.des_facet.map(facet => {
              this.allCategories.add(facet)
            })
          })
          this.categories = Array.from(this.allCategories)
          console.log(this.allCategories)
        })
        .catch(error => {
          this.alert = true
          this.alertMessage = 'Check that your New York Times API Key is correct.'
          console.log(error)
        })
    },
  },
  computed: {
    filteredResults() {
      return this.results.filter( i => {
        return this.selectedCategories.length == 0 ||
          this.selectedCategories.some(v => i.des_facet.includes(v))
      })
    },
  },
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
