<template>
  <q-page class="flex flex-center">
    <div class="chat-container">
      <ChatComponent />
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'
import ChatComponent from 'components/ChatComponent.vue'

export default defineComponent({
  name: 'IndexPage',
  components: {
    ChatComponent
  }
})
</script>