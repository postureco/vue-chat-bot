<template lang="pug">
.qkb-msg-bubble(:class="bubbleClass")
  .qkb-msg-avatar(v-if="message.agent === 'bot'")
    .qkb-msg-avatar__img &nbsp;
  .qkb-msg-img.qkb-msg-img__ext(ref="qkb-msg-img__ext")
  component(
    v-if="componentType",
    :is="componentType",
    :main-data="message"
    :is-latest="isLatest"
    :is-bot="isBot"
  )
    slot(v-for="(_, name) in $slots" :name="name" :slot="name")
  .qkb-msg-bubble__time(v-if="message.createdAt")
    | {{ message.createdAt }}
</template>
<script>
import SingleText from './SingleText'
import ButtonOptions from './ButtonOptions'

export default {
  components: {
    SingleText,
    ButtonOptions
  },

  props: {
    message: {
      type: Object
    },
    isLatest: {
      type: Boolean,
      default: false
    },
    isLastOfAgent: {
      type: Boolean,
      default: false
    },
    imgTarget: {
      type: [String, Boolean],
      default: '.qkb-msg-img__ext'
    }
  },

  computed: {
    isBot () {
      return (this.message.agent === 'bot')
    },
    bubbleClass () {
      const agent = (this.message.agent === 'bot'
        ? 'qkb-msg-bubble--bot'
        : 'qkb-msg-bubble--user')
      return agent + (this.isLatest ? ' qkb-active-msg' : '') + (this.isLastOfAgent ? ' qkb-last-of-agent qkb-last--' + this.message.agent : '')
    },

    // Define the message type and return the specific component
    componentType () {
      let type = ''

      switch (this.message.type) {
        case 'button':
          type = 'ButtonOptions'
          break
        default:
          type = 'SingleText'
      }

      return type
    }
  }
}
</script>
