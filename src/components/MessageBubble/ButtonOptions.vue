<template lang="pug">
.qkb-msg-bubble-component.qkb-msg-bubble-component--button-options
  .qkb-msg-bubble-component__text(v-if="mainData.type === 'text'") {{ mainData.text }}
  .qkb-msg-bubble-component__text(v-if="['html', 'button'].includes(mainData.type)" v-html="mainData.text")
  .qkb-msg-bubble-component__options-wrapper(:class="{'qkb-interacted' : selectedItem !== null}")
    .qkb-mb-button-options__item(
      v-for="(item, index) in mainData.options",
      :class="{ active: selectedItem === item.value }",
      :key="index"
    )
      button.qkb-mb-button-options__btn(
        v-if="item.action === 'postback'",
        @click="selectOption(item)"
        :disabled="disabled"
      )
        span {{ item.text }}
      a.qkb-mb-button-options__btn.qkb-mb-button-options__url(
        v-else,
        :href="item.value"
      )
        span {{ item.text }}
</template>
<script>
import EventBus from '../../helpers/event-bus'

export default {
  props: {
    mainData: {
      type: Object
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },

  data () {
    return {
      selectedItem: null
    }
  },

  methods: {
    selectOption (item) {
      this.selectedItem = item.value
      EventBus.$emit('select-button-option', item, (this.mainData.options.length > 1))
    }
  }
}
</script>
