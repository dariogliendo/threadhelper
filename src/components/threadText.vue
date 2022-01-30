<template lang="pug">
  form(name="threadContent")
    .col
      textarea#contentInput(v-model="content")
      .row.counters(style="justify-content: flex-end;")
        span {{ tweetCount }}
        span {{ charCount }}
</template>

<script>
import {eventBus} from "../main";
export default {
  name: 'threadText',
  props: {
  },
  data: function() {
    return {
      content: '',
      charCount: 0,
      tweetCount: 0,
    }
  },
  methods: {
    update() {
      this.charCount = this.content.length
      if (this.charCount > 280) this.tweetCount = Math.floor(this.charCount / 280)
      eventBus.$emit('contentUpdated', {content: this.content, inputEl: document.getElementById('contentInput')})
    },
  },
  watch: {
    content: function() {
      this.update()
    }
  },
  created() {
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
  textarea {
    width: 100%;
    height: 60px;
  }

  .counters {
    span {
      margin-left: 12px;
    }
  }
</style>
