# Vue News

This app retries the top news from New York Times and a summary of each news. You can also filter by news category. 

You will need to get a New York Times API Key that looks like this: `6c15d80803cd097825fb354684d7aa6d9f7a464b`

## Project setup
```
yarn install
```

To test it locally, run 
```
yarn run serve
```

and go to http://localhost:8080

### Compiles and minifies for production
```
yarn run build
```

Note that this app expects to run with the url `/vue-news/`. To change that, you can modify the **Vue Router** setting's `base` in [App.vue](https://github.com/hanxue/vue-news/blob/master/src/App.vue)

You can test this application at https://hanxue.co/vue-news/
