<template>
  <v-app dark>
    <v-container class="chat-container">
      <v-card class="chat-box">
        <v-card-title class="chat-title">Medical AI Chat</v-card-title>
        <v-card-text class="chat-messages" ref="messageContainer">
          <div
            v-for="(message, index) in messages"
            :key="index"
            :class="['message', message.type]"
          >
            <span class="message-text">{{ message.text }}</span>
          </div>
          <div v-if="isTyping" class="message bot typing">
            <span class="dot"></span>
            <span class="dot"></span>
            <span class="dot"></span>
          </div>
        </v-card-text>
        <v-divider></v-divider>
        <v-card-actions class="input-container">
          <v-text-field 
            class="chat-input"
            label="Describe your symptoms..." 
            v-model="symptoms" 
            outlined
            dense
            hide-details
            append-inner-icon="mdi-send"
            @keyup.enter="submitDiagnosis"
            @click:append-inner="submitDiagnosis"
          ></v-text-field>
        </v-card-actions>
      </v-card>
    </v-container>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      symptoms: '',
      messages: [{ text: "Hello! How can I assist you today?", type: "bot" }],
      isTyping: false
    };
  },
  methods: {
    async submitDiagnosis() {
      if (!this.symptoms.trim()) return;

      this.messages.push({ text: this.symptoms, type: "user" });
      this.symptoms = '';

      this.$nextTick(() => this.scrollToBottom());

      this.isTyping = true;
      setTimeout(() => {
        this.isTyping = false;
        this.messages.push({ text: "Processing your symptoms...", type: "bot" });
        this.$nextTick(() => this.scrollToBottom());
      }, 1200);
    },
    scrollToBottom() {
      this.$nextTick(() => {
        const container = this.$refs.messageContainer;
        if (container) {
          container.scrollTop = container.scrollHeight;
        }
      });
    }
  }
};
</script>

<style>
html, body, #app, .v-application {
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
  background: #101010;
  color: #d0d0d0;
  font-family: 'Orbitron', sans-serif;
}

.chat-container {
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
}

.chat-box {
  width: 95vw;
  height: 90vh;
  background: #151515;
  border-radius: 8px;
  padding: 10px;
  display: flex;
  flex-direction: column;
  border: 1px solid #00ff99;
}

.chat-title {
  text-align: center;
  font-size: 1.5rem;
  font-weight: bold;
  color: #00ff99;
  padding-bottom: 10px;
}

.chat-messages {
  flex-grow: 1;
  overflow-y: auto;
  padding: 15px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  scrollbar-width: thin;
}

.chat-messages::-webkit-scrollbar {
  display: none;
}

.message {
  max-width: 70%;
  padding: 10px 14px;
  border-radius: 12px;
  font-size: 1rem;
  word-wrap: break-word;
  opacity: 0;
  transform: translateY(10px);
  animation: fadeIn 0.3s forwards ease-out;
  border: 1px solid #00ff99;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.user {
  align-self: flex-end;
  background: #00ff99;
  color: black;
  border-radius: 10px 10px 3px 10px;
}

.bot {
  align-self: flex-start;
  background: #222222;
  color: #00ff99;
  border-radius: 10px 10px 10px 3px;
}

.message-text {
  display: block;
}

.input-container {
  display: flex;
  align-items: center;
  width: 100%;
  background: #121212;
  padding: 8px;
  border-top: 1px solid #00ff99;
}

.chat-input {
  flex-grow: 1;
  background: #1a1a1a;
  color: #00ff99;
  border: 1px solid #00ff99;
  border-radius: 6px;
  font-size: 1rem;
}

.typing {
  display: flex;
  gap: 4px;
  align-items: center;
  justify-content: flex-start;
  background: #222;
  border-radius: 10px;
  padding: 8px;
  animation: fadeIn 0.3s forwards ease-out;
}

.dot {
  width: 6px;
  height: 6px;
  background: #00ff99;
  border-radius: 50%;
  animation: typingAnimation 1.5s infinite ease-in-out;
}

.dot:nth-child(1) {
  animation-delay: 0s;
}

.dot:nth-child(2) {
  animation-delay: 0.2s;
}

.dot:nth-child(3) {
  animation-delay: 0.4s;
}

@keyframes typingAnimation {
  0% {
    transform: scale(1);
    opacity: 0.3;
  }
  50% {
    transform: scale(1.2);
    opacity: 1;
  }
  100% {
    transform: scale(1);
    opacity: 0.3;
  }
}
</style>
