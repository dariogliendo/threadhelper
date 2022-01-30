<template lang="pug">
  .col.dividedContent
    p(v-for="tweet in tweets") {{tweet}}
    p {{currentTweet}}
</template>

<script>
const Diff = require('diff')
import {eventBus} from "../main";
export default {
  name: 'contentDivision',
  props: {
  },
  data: function() {
    return {
      content: '',
      tweets: [],
      tweetCheckpoint: 280,
      currentTweet: '',
      lastCheckpoint: 0,
      inputEl: {},
      addedContent: '',
      removedContent: ''
    }
  },
  methods: {
    // divideInTweets() {
    //     if (this.content.length < this.tweetCheckpoint) this.currentTweet = this.content.slice(this.lastCheckpoint, (this.lastCheckpoint + (this.content.length - this.lastCheckpoint)))
    //     if (this.content.length === this.tweetCheckpoint) {
    //       this.tweets.push(this.content.slice(this.lastCheckpoint, this.content.length))
    //       this.currentTweet = ''
    //       this.tweetCheckpoint += 280
    //     }
    // },
    updateTweets() {
      var startPos = this.inputEl.selectionStart;
      if (startPos < 280) var modifiedTweet = 0;
      else for(let i = 0; i < startPos; i+=280) {
        modifiedTweet += 1;
      }
      if (this.tweets && this.tweets.length && startPos < this.lastCheckpoint) this.tweets[modifiedTweet] = this.content.slice(modifiedTweet === 0 ? 0 : (modifiedTweet*280), 280)
    }
  },
  created() {
    if(!window.vue) window.vue = {}
    window.vue.contentDivision = this
    eventBus.$on('contentUpdated', (data) => {
      this.content = data.content
      this.inputEl = data.inputEl
    })
  },
  watch: {
    tweetCheckpoint: function(newValue, oldValue) {
      if (newValue === oldValue) return
      if (this.tweetCheckpoint > 280) this.lastCheckpoint = this.tweetCheckpoint - 280 
    },
    content: function(newValue, oldValue) {
      if (newValue === oldValue) return;
      this.tweets = [];
      let contentDiff = Diff.diffChars(oldValue,newValue)
      this.addedContent = contentDiff.filter((f) => f.added).map((m) => m.value).join('')
      this.removedContent = contentDiff.filter((f) => f.removed).map((m) => m.value).join('')
      for(let i = 0; i < this.content.length; i+=280) {
        let sliceStart = i === 0 ? 0 : i
        let currentContent = this.content.slice(sliceStart, sliceStart + 280)
        if (currentContent.length%i === 0 || (currentContent.length === 280 && i === 0)) this.tweets.push(currentContent)
        else this.currentTweet = currentContent
      }
    }
  }
}
</script>

<style lang="less" scoped>
  .dividedContent {
    p {
      white-space: pre-wrap;
      line-break: anywhere;
      text-align: left;
    }
  }
</style>