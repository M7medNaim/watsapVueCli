<template>
  <div class="chat-input w-100 position-relative bg-body-secondary p-2">
    <div class="row m-0 p-0">
      <div
        class="col-2 col-md-1 d-flex justify-content-start align-items-center pt-1"
      >
        <div class="emoje">
          <button
            type="submit"
            @click="toggleEmoji"
            class="border-0 bg-transparent"
            aria-label="Emojis"
          >
            <i
              class="fa-regular fa-face-smile fs-4 text-body-secondary ms-2"
            ></i>
          </button>
          <!-- all Emojis -->
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
        </div>
        <!-- upload_file Icon -->
        <div class="upload_file cursor-pointer">
          <label for="file" class="upload-label">
            <i
              class="fa-solid fa-paperclip upload-icon fs-4 text-body-secondary"
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
      </div>
      <!-- input send message -->
      <div class="col-9 col-md-10">
        <input
          type="text"
          ref="messageInput"
          placeholder="Type a message"
          id="inputMassege"
          class="rounded-2 fs-6 border-0 py-2 px-4 shadow-lg w-100"
          style="outline: none"
          v-model="newMessage"
        />
      </div>
      <!-- icon send & voice Message -->
      <div class="col-1 d-flex justify-content-center align-items-center pt-1">
        <button
          type="submit"
          class="submitMsg position-relative border-0 bg-transparent"
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
    </div>
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
</style>
