<template>
  <div id="app">
    <div>
      <github-api url="reposdfsdfs/vouill/vue-fdsfsdfsdfsdfsdf/commits" key-path="vue-fdsfsdfsdfsdfsdf"/>
      <h2>error</h2>
      <tree-view :data="error" :options="{maxDepth: 1}"/>
    </div>
    <div>
      <json-api :params="{postId: post}" url="posts" key-path="json"/>
      <h2>json api w/ param</h2>
      <button @click="post++">add post nb</button>
      <tree-view :data="json" :options="{maxDepth: 1}"/>
    </div>

    <div>
      <h2>json api Post</h2>
      <tree-view :data="postPost" :options="{maxDepth: 1}"/>
      <button @click="sendPostReq">Post request</button>
    </div>
  </div>
</template>
<style>
  * {
    font-family: 'Roboto', sans-serif;
  }
</style>
<script>
  import { mapState } from 'vuex'
  import pluginActions from 'plugin/actions'
  import { getApiState } from './plugin'

  export default {
    name: 'App',
    data: function () {
      return ({
        post: 1
      })
    },
    computed: mapState({
      repo: getApiState('vue-bulma-components'),
      geb: getApiState('vue-geb'),
      error: getApiState('vue-fdsfsdfsdfsdfsdf'),
      json: getApiState('json'),
      jsonStatus: getApiState('json', ['status']),
      jsonResp: getApiState('json', ['resp']),
      postPost: getApiState('postPost')
    }),
    created: function () {
      // check the vuex progammaticGet in your vue dev tools
      this.$store.dispatch(pluginActions.request, {
        baseURL: 'https://jsonplaceholder.typicode.com',
        method: 'GET',
        url: 'posts',
        keyPath: 'progammaticGet'
      })
    },
    methods: {
      clearApiHandlerComponent: function (keyPath) {
        this.$store.dispatch(pluginActions.clear, keyPath)
      },
      sendPostReq: function () {
        this.$store.dispatch(pluginActions.request, {
          requestConfig: { baseURL: 'https://jsonplaceholder.typicode.com' },
          method: 'POST',
          url: 'posts',
          data: { title: 'foo', userId: 2, body: 'bar' },
          keyPath: 'postPost',
          onSuccess: {
            dispatchAction: {
              type: pluginActions.request,
              payload: {
                baseURL: 'https://jsonplaceholder.typicode.com',
                method: 'GET',
                url: 'posts',
                keyPath: 'json'
              }
            }
          }
        })
      }
    },
}
</script>

