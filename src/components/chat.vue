<template>
  <div class="chat-container">
    <div class="chat-header">
      <span class="neon-text">New World AI</span>
      <button class="close-btn" @click="closeChat">×</button>
    </div>
    <div class="chat-messages" ref="chatMessages">
      <div
        v-for="(message, index) in messages"
        :key="index"
        :class="['chat-message', message.role]"
      >
        <span>{{ message.content }}</span>
      </div>
    </div>
    <div class="chat-input">
      <input
        type="text"
        v-model="userInput"
        placeholder="Type your message..."
        @keyup.enter="sendMessage"
      />
      <button @click="sendMessage" class="send-btn">
        <ion-icon name="send-outline"></ion-icon>
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: 'Chat',
  data() {
    return {
      userInput: "",
      messages: [
        {
          role: "system",
          content: "Hello! I am New World AI. How can I assist you today?",
        },
      ],
      apiKey:"you key",
    };
  },
  methods: {
    async sendMessage() {
      if (!this.userInput.trim()) return;
      
      const userMessage = this.userInput.trim();
      this.messages.push({ role: "user", content: userMessage });
      this.userInput = "";
      this.scrollToBottom();

      try {
        const response = await axios.post(
          "https://api.openai.com/v1/chat/completions",
          {
            model: "gpt-3.5-turbo",
            messages: this.messages.map((msg) => ({
              role: msg.role === "system" ? "system" : 
                    msg.role === "user" ? "user" : "assistant",
              content: msg.content,
            })),
            temperature: 0.7,
            max_tokens: 1000,
          },
          {
            headers: {
              "Content-Type": "application/json",
              Authorization: `Bearer ${this.apiKey}`,
            },
          }
        );

        if (response.data && response.data.choices && response.data.choices[0]) {
          const gptMessage = response.data.choices[0].message;
          this.messages.push({ role: "assistant", content: gptMessage.content });
        } else {
          throw new Error("Invalid response format");
        }
      } catch (error) {
        console.error("Error calling New World AI API:", error);
        let errorMessage = "Sorry, there was an error. ";
        
        if (error.response) {
         
          if (error.response.status === 401) {
            errorMessage += "Invalid API key or authentication error.";
          } else if (error.response.status === 429) {
            errorMessage += "Rate limit exceeded. Please try again later.";
          } else {
            errorMessage += error.response.data.error?.message || "Please try again later.";
          }
        } else if (error.request) {
          
          errorMessage += "Network error. Please check your connection.";
        }
        
        this.messages.push({
          role: "assistant",
          content: errorMessage,
        });
      } finally {
        this.scrollToBottom();
      }
    },
    scrollToBottom() {
      this.$nextTick(() => {
        const chatMessages = this.$refs.chatMessages;
        chatMessages.scrollTop = chatMessages.scrollHeight;
      });
    },
    closeChat() {
      this.$parent.toggleChat();
    }
  },
};
</script>

<style scoped>
.chat-container {
  width: 400px;
  height: 600px;
  background: rgba(20, 22, 34, 0.95);
  display: flex;
  flex-direction: column;
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0 0 20px rgba(0, 255, 255, 0.2),
              0 0 40px rgba(0, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(0, 255, 255, 0.1);
  animation: container-glow 4s infinite alternate;
}

@keyframes container-glow {
  from {
    box-shadow: 0 0 20px rgba(0, 255, 255, 0.2),
                0 0 40px rgba(0, 255, 255, 0.1);
  }
  to {
    box-shadow: 0 0 25px rgba(0, 255, 255, 0.3),
                0 0 50px rgba(0, 255, 255, 0.2);
  }
}

.chat-header {
  background: rgba(25, 27, 40, 0.95);
  padding: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid rgba(0, 255, 255, 0.2);
}

.neon-text {
  color: #fff;
  font-size: 18px;
  font-weight: 600;
  text-shadow: 0 0 5px rgba(0, 255, 255, 0.5),
               0 0 10px rgba(0, 255, 255, 0.3);
  animation: text-glow 2s infinite alternate;
}

@keyframes text-glow {
  from {
    text-shadow: 0 0 5px rgba(0, 255, 255, 0.5),
                 0 0 10px rgba(0, 255, 255, 0.3);
  }
  to {
    text-shadow: 0 0 7px rgba(0, 255, 255, 0.7),
                 0 0 15px rgba(0, 255, 255, 0.5);
  }
}

.close-btn {
  background: none;
  border: none;
  color: #0ff;
  font-size: 24px;
  cursor: pointer;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  transition: all 0.3s ease;
  text-shadow: 0 0 5px rgba(0, 255, 255, 0.5);
}

.close-btn:hover {
  background: rgba(0, 255, 255, 0.1);
  text-shadow: 0 0 10px rgba(0, 255, 255, 0.8);
}

.chat-messages {
  flex: 1;
  padding: 20px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.chat-message {
  padding: 12px 16px;
  border-radius: 12px;
  max-width: 85%;
  word-wrap: break-word;
  line-height: 1.4;
  position: relative;
  animation: messageAppear 0.3s ease;
}

@keyframes messageAppear {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.chat-message.user {
  background: rgba(0, 255, 255, 0.1);
  color: #fff;
  align-self: flex-end;
  border-bottom-right-radius: 4px;
  border: 1px solid rgba(0, 255, 255, 0.2);
  box-shadow: 0 0 10px rgba(0, 255, 255, 0.1);
}

.chat-message.assistant {
  background: rgba(255, 255, 255, 0.05);
  color: #0ff;
  align-self: flex-start;
  border-bottom-left-radius: 4px;
  border: 1px solid rgba(0, 255, 255, 0.1);
  text-shadow: 0 0 5px rgba(0, 255, 255, 0.3);
}

.chat-message.system {
  background: rgba(0, 255, 255, 0.05);
  color: #0ff;
  align-self: center;
  font-style: italic;
  font-size: 0.9em;
  text-shadow: 0 0 5px rgba(0, 255, 255, 0.3);
}

.chat-input {
  display: flex;
  padding: 20px;
  background: rgba(25, 27, 40, 0.95);
  gap: 10px;
  border-top: 1px solid rgba(0, 255, 255, 0.2);
}

.chat-input input {
  flex: 1;
  padding: 12px 16px;
  background: rgba(0, 255, 255, 0.05);
  color: #0ff;
  border: 1px solid rgba(0, 255, 255, 0.2);
  border-radius: 8px;
  font-size: 14px;
  transition: all 0.3s ease;
}

.chat-input input:focus {
  outline: none;
  background: rgba(0, 255, 255, 0.1);
  border-color: rgba(0, 255, 255, 0.4);
  box-shadow: 0 0 10px rgba(0, 255, 255, 0.2);
}

.chat-input input::placeholder {
  color: rgba(0, 255, 255, 0.5);
}

.send-btn {
  background: rgba(0, 255, 255, 0.1);
  border: 1px solid rgba(0, 255, 255, 0.3);
  color: #0ff;
  padding: 12px;
  border-radius: 8px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
  box-shadow: 0 0 10px rgba(0, 255, 255, 0.1);
}

.send-btn:hover {
  background: rgba(0, 255, 255, 0.2);
  border-color: rgba(0, 255, 255, 0.5);
  box-shadow: 0 0 15px rgba(0, 255, 255, 0.2);
  transform: translateY(-1px);
}

.send-btn ion-icon {
  font-size: 20px;
  filter: drop-shadow(0 0 2px rgba(0, 255, 255, 0.5));
}

::-webkit-scrollbar {
  width: 6px;
}

::-webkit-scrollbar-track {
  background: rgba(0, 255, 255, 0.05);
}

::-webkit-scrollbar-thumb {
  background: rgba(0, 255, 255, 0.2);
  border-radius: 3px;
}

::-webkit-scrollbar-thumb:hover {
  background: rgba(0, 255, 255, 0.3);
}
</style>
