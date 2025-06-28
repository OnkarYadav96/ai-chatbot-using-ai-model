/* eslint-disable */
<template>
  <div class="chat-container">
    <div class="messages">
      <div v-for="(message, index) in messages" :key="index" :class="message.sender">
        {{ message.text }}
      </div>
    </div>
    <div class="input-container">
      <input v-model="newMessage" @keyup.enter="sendMessage" placeholder="Type a message..." />
      <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<script>
import { useChatStore } from '../stores/chatStore';
import {ref} from "vue";

export default {
  setup() {
    const chatStore = useChatStore();
    const newMessage = ref('');

    const sendMessage = () => {
      if (newMessage.value.trim() !== '') {
        chatStore.addMessage({ text: newMessage.value, sender: 'user' });
        newMessage.value = '';
        // Here you would call your AI service to get a response
        // For simplicity, we'll just add a dummy response
        setTimeout(() => {
          chatStore.addMessage({ text: 'This is a dummy response.', sender: 'bot' });
        }, 500);
      }
    };

    return {
      messages: chatStore.messages,
      newMessage,
      sendMessage,
    };
  },
};
</script>

<style scoped>
.chat-container {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.messages {
  flex: 1;
  overflow-y: auto;
  padding: 10px;
}

.input-container {
  display: flex;
  padding: 10px;
}

input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 10px;
  margin-left: 10px;
  border: none;
  background-color: #007bff;
  color: white;
  border-radius: 4px;
  cursor: pointer;
}

.user {
  text-align: right;
}

.bot {
  text-align: left;
}
</style>