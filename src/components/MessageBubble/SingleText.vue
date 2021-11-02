<template lang="pug">
.qkb-msg-bubble-component.qkb-msg-bubble-component--single-text
  .qkb-msg-bubble-component__text(
    v-if="mainData.type === 'text'"
    :class="{ 'has-msg-action' : hasMsgAction && isLatest}"
    ) {{ mainData.text }}
    slot(name="activeMsgAction" v-if="isLatest")
  .qkb-msg-bubble-component__text(v-if="['html', 'button'].includes(mainData.type)" v-html="mainData.text")
</template>
<script>
export default {
  props: {
    mainData: {
      type: Object
    },
    isLatest: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    hasMsgAction () {
      console.log('slots', this.$scopedSlots.activeMsgAction)
      return this.$scopedSlots.activeMsgAction
    }
  }
}
</script>
