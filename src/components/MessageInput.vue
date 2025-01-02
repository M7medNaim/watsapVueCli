<template>
  <div class="chat-input pb-4">
    <button type="submit" @click="toggleEmoji">
      <i class="fa-regular fa-face-smile"></i>
    </button>
    <div class="upload_file">
      <label for="file" class="upload-label">
        <i class="fa-solid fa-paperclip upload-icon pt-2"></i>
      </label>
      <form action="" autocomplete="off">
        <input type="file" id="file" class="file-upload" autocomplete="off" />
      </form>
    </div>
    <input
      type="text"
      ref="messageInput"
      placeholder="Type a message"
      id="inputMassege"
      v-model="newMessage"
    />
    <button type="submit" class="submitMsg" @click="sendMessage">
      <i ref="voiceIcon" class="fa-solid fa-microphone" id="voiceIcon"></i>
      <i ref="sendIcon" class="fa-solid fa-paper-plane" id="sendIcon"></i>
    </button>
  </div>
</template>

<script>
export default {
  name: "MessageInput",
  data() {
    return {
      newMessage: "",
    };
  },
  mounted() {
    let messageInput = this.$refs.messageInput;
    let voiceIcon = this.$refs.voiceIcon;
    let sendIcon = this.$refs.sendIcon;

    messageInput.addEventListener("input", () => {
      if (messageInput.value.trim() !== "") {
        voiceIcon.style.display = "none";
        sendIcon.style.display = "block";
        sendIcon.style.position = "relative";
      } else {
        voiceIcon.style.display = "block";
        sendIcon.style.display = "none";
      }
    });
  },
  methods: {
    sendMessage() {
      if (this.newMessage.trim() !== "") {
        this.$emit("send-message", this.newMessage);
        this.newMessage = "";
      }
    },
    toggleEmoji() {},
  },
};
</script>
<style scoped>
/* chat input */
.right-side .chat-input {
  position: relative;
  width: 100%;
  height: 60px;
  background: #f0f0f0;
  padding: 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.right-side .chat-input i {
  cursor: pointer;
  font-size: 1.5rem;
  color: #51585c;
}

.right-side .chat-input i:nth-child(1) {
  margin-right: 15px;
}

.right-side .chat-input input {
  position: relative;
  width: 90%;
  margin: 0 20px 7px 20px;
  padding: 10px 20px;
  border: none;
  outline: none;
  border-radius: 30px;
  font-size: 1rem;
}

.right-side .chat-input .file-upload {
  display: none;
}

.right-side .chat-input .upload-label {
  cursor: pointer;
}

.right-side .chat-input .upload-icon {
  width: 40px;
  height: 40px;
}

.right-side .chat-input .submitMsg {
  position: relative;
}

.right-side .chat-input .submitMsg i.fa-paper-plane {
  position: absolute;
  top: 0;
  display: none;
}
</style>
