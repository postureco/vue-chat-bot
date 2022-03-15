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
      :isLastOfAgent="item.lastOfAgent || false"
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
    },

    autoScroll: {
      type: Boolean,
      default: true
    },

    imgTarget: {
      type: [String, Boolean]
    }
  },

  data () {
    return {
      msgCount: 0,
      currentImg: null,
      imgDuration: 0 // 0: infinite; 1+ remove after X bot messages
    }
  },

  mounted () {
    this.updateScroll()
  },

  computed: {
    latestMessage () {
      return this.mainData[this.mainData.length - 1] || null
    }
  },

  watch: {
    mainData: function (newVal) {
      this.$nextTick(() => {
        this.updateUI()
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

    updateUI () {
      /* eslint-disable no-console */
      console.log('updating ui')
      if (this.autoScroll) {
        this.updateScroll()
      }

      // check for msg images
      if (this.latestMessage && this.latestMessage.image) {
        // force remove
        if (this.latestMessage.image.url) {
          this.setImage(this.latestMessage.image)
        } else {
          this.removeImage() // force empty image container if url is omitted
        }
      } else if (this.latestMessage.agent === 'bot') {
        /* eslint-disable no-console */
        console.log('latest', this.latestMessage)

        // update msg counter and remove images as needed
        if (this.imgDuration && this.imgDuration > 1) {
          this.imgDuration--
        } else if (this.imgDuration === 1) {
          // remove image
          this.removeImage()
          this.imgDuration = 0
        }
      }
    },

    setImage (img) {
      if (img && img.url && this.imgTarget) {
        const targets = document.querySelectorAll(this.imgTarget)
        if (targets && targets.length) {
          targets[targets.length - 1].innerHTML = `<img class="qkb-msg-img-src" src="${img.url}" />`

          this.imgDuration = img.msgDuration || 0 // 0 will remain until the next image removes it
        }
      }
    },

    removeImage () {
      if (this.imgTarget) {
        const targets = document.querySelectorAll(this.imgTarget)
        if (targets && targets.length) {
          targets[targets.length - 1].innerHTML = ''
        }
      }
    },

    updateScroll () {
      const contentElm = this.$refs.boardContent
      const offsetHeight = this.$refs.boardBubbles.offsetHeight

      contentElm.scrollTop = offsetHeight
    }
  }
}
</script>
