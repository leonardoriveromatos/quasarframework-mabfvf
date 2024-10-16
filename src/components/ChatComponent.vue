<template>
  <div class="chat-component">
    <div class="messages" ref="messagesContainer">
      <div v-for="(message, index) in messages" :key="index" :class="['message', message.type]">
        {{ message.text }}
        <div v-if="message.type === 'bot'" class="feedback">
          <q-btn flat round color="positive" icon="thumb_up" @click="provideFeedback(index, 'like')" />
          <q-btn flat round color="negative" icon="thumb_down" @click="provideFeedback(index, 'dislike')" />
        </div>
      </div>
    </div>
    <div class="chat-input">
      <q-input
        v-model="userInput"
        :placeholder="$t('chat.placeholder')"
        @keyup.enter="sendMessage"
        outlined
        dense
      >
        <template v-slot:after>
          <q-btn round dense flat icon="send" @click="sendMessage" />
        </template>
      </q-input>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted, nextTick } from 'vue'
import { useQuasar } from 'quasar'
import { useI18n } from 'vue-i18n'

export default defineComponent({
  name: 'ChatComponent',
  setup() {
    const $q = useQuasar()
    const { t } = useI18n()
    const messages = ref([])
    const userInput = ref('')
    const messagesContainer = ref(null)

    const sendMessage = () => {
      if (userInput.value.trim() === '') return

      messages.value.push({ type: 'user', text: userInput.value })
      userInput.value = ''

      // Simular respuesta del bot
      setTimeout(() => {
        messages.value.push({ type: 'bot', text: t('chat.botWelcome') })
        scrollToBottom()
      }, 1000)
    }

    const scrollToBottom = () => {
      nextTick(() => {
        if (messagesContainer.value) {
          messagesContainer.value.scrollTop = messagesContainer.value.scrollHeight
        }
      })
    }

    const provideFeedback = (index, type) => {
      console.log(`Feedback provided for message ${index}: ${type}`)
      $q.notify({
        message: type === 'like' ? t('chat.likeFeedback') : t('chat.dislikeFeedback'),
        color: type === 'like' ? 'positive' : 'negative'
      })
    }

    onMounted(() => {
      messages.value.push({ type: 'bot', text: t('chat.botWelcome') })
    })

    return {
      t,
      messages,
      userInput,
      sendMessage,
      messagesContainer,
      provideFeedback
    }
  }
})
</script>

<style lang="scss" scoped>
.chat-component {
  height: calc(100vh - 100px);
  display: flex;
  flex-direction: column;
}

.messages {
  flex-grow: 1;
  overflow-y: auto;
  padding: 20px;
}

.feedback {
  display: flex;
  justify-content: flex-end;
  margin-top: 5px;
}
</style>