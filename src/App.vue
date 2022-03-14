<template lang="pug">
#app
  img(
    alt="Vue Bot UI",
    src="./assets/logo.png"
    style="z-index: 100"
  )
  #test-img-target
  div(style="width: 100%;display:flex; justify-content: center")
    VueChatBot(
      :options="botOptions",
      :messages="messageData",
      :bot-typing="botTyping",
      :input-disable="inputDisable",
      :input-password="inputPassword"
      :is-open="true",
      :showImages="true"
      imgTarget="#test-img-target"
      @init="botStart",
      @msg-send="msgSend",
    )
      template(
        v-slot:activeMsgAction
        v-if="test"
      ) &nbsp;⇢
</template>
<script>
import BotIcon from './assets/icons/bot.png'
import { VueChatBot } from './vue-chat-bot'
import { messageService } from './helpers/message'

export default {
  components: {
    BotIcon,
    VueChatBot
  },

  data () {
    return {
      messageData: [],
      botTyping: false,
      inputDisable: false,
      inputPassword: false,
      botOptions: {
        presentation: true,
        botAvatarImg: BotIcon,
        boardContentBg: '#f2e9f7',
        msgBubbleBgBot: '#fff',
        inputPlaceholder: 'Reply here...',
        inputDisableBg: '#fff',
        inputDisablePlaceholder: 'Tap a button above to respond',
        topRight: false,
        movable: true
      }
    }
  },
  computed: {
    test () {
      return true
    }
  },
  methods: {
    botStart () {
      // Get token if you want to build a private bot
      // Request first message here

      // Fake typing for the first message
      this.botTyping = true
      // setTimeout(() => {
      //   this.botTyping = false
      //   this.messageData.push({
      //     agent: 'bot',
      //     type: 'button',
      //     text: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
      //     disableInput: true,
      //     options: [
      //       {
      //         'text': 'option 1',
      //         'value': 'a',
      //         'action': 'postback'
      //       },
      //       {
      //         'text': 'option 2',
      //         'value': 'b',
      //         'action': 'postback'
      //       }
      //     ]
      //   })
      // }, 1000)

      setTimeout(() => {
        this.botTyping = false
        this.messageData.push({
          agent: 'bot',
          type: 'text',
          text: 'Lorem ipsum dolor sit amet.',
          image: 'https://i.imgur.com/hp7Fp1U.gif'
        })
      }, 1000)
    },

    msgSend (value) {
      // Push the user's message to board
      this.messageData.push({
        agent: 'user',
        type: 'text',
        text: value.text
      })

      this.getResponse()
    },

    // Submit the message from user to bot API, then get the response from Bot
    getResponse () {
      // Loading
      this.botTyping = true

      // Post the message from user here
      // Then get the response as below

      // Create new message from fake data
      messageService.createMessage()
        .then((response) => {
          const replyMessage = {
            agent: 'bot',
            ...response
          }
          this.inputDisable = response.disableInput
          this.inputPassword = response.password
          this.messageData.push(replyMessage)
          // this.messageData = [replyMessage]

          // finish
          this.botTyping = false
        })
    }
  }
}
</script>
<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  // text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
