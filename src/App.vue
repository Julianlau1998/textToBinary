<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
  export default {
    computed: {
      iosLiteApp () {
        return window.webkit && window.webkit.messageHandlers
      }
    },
    methods: {
      showInterstitial() {
        if (this.iosLiteApp) {
          window.webkit.messageHandlers.showInterstitial.postMessage({
            "message": 'showInterstitial'
          })
        }
      }
    }
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
