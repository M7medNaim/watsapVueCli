<template>
  <div
    class="chat-input pb-4 d-flex justify-content-between align-items-center w-100 position-relative"
  >
    <button type="submit" @click="toggleEmoji" class="pt-2" aria-label="Emojis">
      <i class="fa-regular fa-face-smile"></i>
    </button>
    <div
      v-if="isEmojiVisible"
      class="emoji-picker bg-white rounded-1 border border-1 border-dark-subtle py-2 px-2 d-flex flex-wrap gap-3 w-25"
    >
      <span @click="addEmoji('😀')">😀</span>
      <span @click="addEmoji('😂')">😂</span>
      <span @click="addEmoji('😍')">😍</span>
      <span @click="addEmoji('😎')">😎</span>
      <span @click="addEmoji('😢')">😢</span>
      <span @click="addEmoji('😡')">😡</span>
      <span @click="addEmoji('😀')">😀</span>
      <span @click="addEmoji('😀')">😀</span>
      <span @click="addEmoji('😂')">😂</span>
      <span @click="addEmoji('😍')">😍</span>
      <span @click="addEmoji('😎')">😎</span>
      <span @click="addEmoji('😢')">😢</span>
      <span @click="addEmoji('😡')">😡</span>
      <span @click="addEmoji('😀')">😀</span>
      <span @click="addEmoji('😀')">😀</span>
      <span @click="addEmoji('😂')">😂</span>
      <span @click="addEmoji('😍')">😍</span>
      <span @click="addEmoji('😎')">😎</span>
      <span @click="addEmoji('😢')">😢</span>
      <span @click="addEmoji('😡')">😡</span>
      <span @click="addEmoji('😀')">😀</span>
      <span @click="addEmoji('😍')">😍</span>
      <span @click="addEmoji('😎')">😎</span>
      <span @click="addEmoji('😢')">😢</span>
    </div>
    <div class="upload_file pt-2">
      <label for="file" class="upload-label">
        <i class="fa-solid fa-paperclip upload-icon pt-2"></i>
      </label>
      <form action="" autocomplete="off">
        <input
          type="file"
          id="file"
          class="file-upload position-relative d-none"
          autocomplete="off"
          aria-label="uploadFile"
        />
      </form>
    </div>
    <input
      type="text"
      ref="messageInput"
      placeholder="Type a message"
      id="inputMassege"
      v-model="newMessage"
    />
    <button
      type="submit"
      class="submitMsg pt-2 position-relative"
      @click="sendMessage"
      aria-label="sendMessage && voiceMessage"
    >
      <i
        ref="voiceIcon"
        class="fa-solid fa-microphone d-block"
        id="voiceIcon"
        v-if="newMessage.trim() === ''"
      ></i>
      <i
        ref="sendIcon"
        class="fa-solid fa-paper-plane d-block position-relative"
        id="sendIcon"
        v-if="newMessage.trim() !== ''"
      ></i>
    </button>
  </div>
</template>

<script>
export default {
  name: "MessageInput",
  data() {
    return {
      newMessage: "",
      isEmojiVisible: false,
    };
  },
  methods: {
    sendMessage() {
      if (this.newMessage.trim() !== "") {
        this.$emit("send-message", this.newMessage);
        this.newMessage = "";
      }
    },
    toggleEmoji() {
      this.isEmojiVisible = !this.isEmojiVisible;
    },
    addEmoji(emoji) {
      this.newMessage += emoji;
      this.isEmojiVisible = false;
    },
  },
};
</script>
<style scoped>
/* list emoji style */
.emoji-picker {
  position: absolute;
  bottom: 60px;
  right: 0;
  font-size: 20px;
  text-align: center;
}

.emoji-picker span {
  cursor: pointer;
  width: 10%;
  transition: transform 0.2s;
}

.emoji-picker span:hover {
  transform: scale(1.2);
}

/* chat input */
.right-side .chat-input {
  height: 60px;
  background: #f0f0f0;
  padding: 15px;
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
  width: 90%;
  margin: 8px 20px 0px 15px;
  padding: 10px 20px;
  border: none;
  outline: none;
  border-radius: 10px;
  font-size: 1rem;
  box-shadow: 2px 2px 5px rgba(14, 14, 14, 0.1);
}

.right-side .chat-input .upload-label {
  cursor: pointer;
}

.right-side .chat-input .upload-icon {
  width: 40px;
  height: 40px;
}

.right-side .chat-input .submitMsg i.fa-paper-plane {
  position: absolute;
  top: 0;
}
</style>
