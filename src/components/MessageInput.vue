<template>
  <div
    class="chat-input pb-4 d-flex justify-content-between align-items-center w-100 position-relative bg-body-secondary p-3"
  >
    <button type="submit" @click="toggleEmoji" class="pt-2" aria-label="Emojis">
      <i class="fa-regular fa-face-smile fs-4 text-body-secondary ms-3"></i>
    </button>
    <div
      v-if="isEmojiVisible"
      class="emoji-picker bg-white rounded-1 border border-1 border-dark-subtle py-2 px-2 d-flex flex-wrap gap-3 w-25 position-absolute fs-6 text-center end-0 bottom-100"
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
        <i
          class="fa-solid fa-paperclip upload-icon pt-2 fs-4 text-body-secondary"
        ></i>
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
      class="rounded-2 fs-6 border-0 py-2 px-4 shadow-lg mt-2 ms-2"
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
        class="fa-solid fa-microphone d-block fs-4 text-body-secondary"
        id="voiceIcon"
        v-if="newMessage.trim() === ''"
      ></i>
      <i
        ref="sendIcon"
        class="fa-solid fa-paper-plane d-block position-relative fs-4 text-body-secondary"
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
}

.right-side .chat-input input {
  width: 90%;
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
