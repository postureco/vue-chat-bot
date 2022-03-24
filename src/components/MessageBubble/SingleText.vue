<template lang="pug">
.qkb-msg-bubble-component.qkb-msg-bubble-component--single-text
  .qkb-msg-img.qkb-msg-img__int(ref="qkb-msg-img__int")
  .qkb-msg-bubble-component__text(
    v-if="mainData.type === 'text'"
    :class="{ 'has-msg-action' : hasMsgAction && isLatest && isBot}"
    ) {{ mainData.text }}
    slot(name="activeMsgAction" v-if="isLatest && isBot")
  .qkb-msg-bubble-component__text(
      v-if="['html', 'button'].includes(mainData.type)"
      :class="{ 'has-msg-action' : hasMsgAction && isLatest && isBot}"
      v-html="mainData.text")
      slot(name="activeMsgAction" v-if="isLatest && isBot")

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
    },
    isBot: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    hasMsgAction () {
      return this.$scopedSlots.activeMsgAction
    }
  }
}
</script>
