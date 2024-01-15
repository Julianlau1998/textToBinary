<template>
  <div>
    <topNav @fileInput="fileInput" @share="shareFile" />
    <Editor @click="addClick" :inputFile="inputFile" :share="share" />
  </div>
</template>

<script>
import Editor from '@/components/Editor.vue'
import topNav from '@/components/TopNav.vue'

export default {
  name: 'Home',
  components: {
    Editor,
    topNav
  },
  data () {
    return {
      inputFile: '',
      share: false
    }
  },
  computed: {
    iosLiteApp () {
      return window.webkit && window.webkit.messageHandlers
    }
  },
  created () {
    this.clicks = parseInt(localStorage.getItem('clicks'))
    if (this.clicks == null || isNaN(this.clicks)) {
      this.clicks = 0
    }
  },
  methods: {
    fileInput (file) {
      this.inputFile = file
    },
    shareFile () {
      this.share = true
      setTimeout(() => {
        this.share = false
      }, 300)
    },
    showInterstitial() {
      if (this.iosLiteApp) {
        window.webkit.messageHandlers.showInterstitial.postMessage({
          "message": 'showInterstitial'
        })
      }
    },
    addClick () {
      this.clicks += 1
      localStorage.setItem('clicks', this.clicks)

      if (this.clicks >= 5) {
        this.showInterstitial()
        this.adShown = true
        localStorage.setItem('clicks', '0')
        this.clicks = 0
      }
    }
  }
}
</script>
