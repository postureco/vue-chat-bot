<template lang="pug">
.qkb-board-action(
  :class="actionClass"
)
  .qkb-board-action__wrapper
    .qkb-board-action__msg-box
      input.qkb-board-action__input(
        :type="inputType",
        v-model="messageText",
        ref="qkbMessageInput",
        :disabled="inputDisable",
        :placeholder="inputPlaceholder",
        @keydown.enter="sendMessage",
      )
      .qkb-board-action__disable-text(
        v-if="inputDisablePlaceholder && inputDisable"
      )
        span {{ inputDisablePlaceholder }}
    .qkb-board-action__extra
      slot(name="actions")
      button.qkb-action-item.qkb-action-item--send(@click="sendMessage")
        slot(name="sendButton")
          IconSend.qkb-action-icon.qkb-action-icon--send
</template>
<script>
import IconSend from '../../assets/icons/send.svg'

export default {
  components: {
    IconSend
  },

  props: {
    inputPlaceholder: {
      type: String
    },

    inputDisablePlaceholder: {
      type: String
    },

    inputDisable: {
      type: Boolean,
      default: false
    },

    inputPassword: {
      type: Boolean,
      default: false
    },
    msgCount: {
      type: Number,
      default: 0
    }
  },

  data () {
    return {
      messageText: null
    }
  },

  computed: {
    actionClass () {
      const actionClasses = []

      if (this.inputDisable) {
        actionClasses.push('qkb-board-action--disabled')
      }

      if (this.messageText) {
        actionClasses.push('qkb-board-action--typing')
      }

      // TODO: sending

      return actionClasses
    },

    inputType () {
      return (this.inputPassword ? 'password' : 'text')
    }
  },

  mounted () {
    this.$refs.qkbMessageInput.focus()
  },

  // focus text field on new messages (if appropriate)
  watch: {
    msgCount (newVal) {
      this.$nextTick(() => { this.$refs.qkbMessageInput.focus() })
    }
  },

  methods: {
    sendMessage () {
      if (this.messageText) {
        this.$emit('msg-send', { text: this.messageText })
        this.messageText = null
      }
    }
  }
}
</script>
