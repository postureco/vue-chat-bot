<template lang="pug">
.qkb-board-content(ref="boardContent")
  .qkb-board-content__bubbles(
    ref="boardBubbles"
  )
    message-bubble(
      v-for="(item, index) in mainData",
      :key="index",
      :message="item",
      :isLatest="(index == mainData.length - 1 ? true : false)"
    )
      slot(v-for="(_, name) in $slots" :name="name" :slot="name")
    .qkb-board-content__bot-typing(v-if="botTyping")
      slot(name="botTyping")
        message-typing-slim
</template>

<script>
import MessageBubble from '../MessageBubble/Main'
// import MessageTyping from '../MessageBubble/Typing'
import MessageTypingSlim from '../MessageBubble/TypingSlim'

export default {
  components: {
    MessageBubble,
    // MessageTyping
    MessageTypingSlim
  },

  props: {
    mainData: {
      type: Array,
      required: true
    },

    botTyping: {
      type: Boolean,
      default: false
    }
  },

  mounted () {
    this.updateScroll()
  },

  watch: {
    mainData: function (newVal) {
      this.$nextTick(() => {
        this.updateScroll()
      })
    }

    // // uncomment this if using the traditional avatar + "..."
    // // typing indicator to force scroll
    // botTyping: function (newVal, oldVal) {
    //   if (newVal === true) {
    //     this.$nextTick(() => {
    //       this.updateScroll()
    //     })
    //   }
    // }
  },

  methods: {
    updateScroll () {
      const contentElm = this.$refs.boardContent
      const offsetHeight = this.$refs.boardBubbles.offsetHeight

      contentElm.scrollTop = offsetHeight
    }
  }
}
</script>
