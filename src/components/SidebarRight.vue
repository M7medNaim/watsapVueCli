<template>
  <div class="col-0 col-md-8 boxChat" v-if="selectedChat">
    <div class="right-side">
      <div class="header">
        <div class="user-details">
          <div class="user-imgBx">
            <img
              :src="selectedChat.img"
              alt=""
              class="img-fluid rounded-circle"
            />
          </div>
          <h5 class="pt-1">
            {{ selectedChat.name }} <br />
            <span>Online</span>
          </h5>
        </div>
      </div>

      <div class="chatBx">
        <p class="date">اليوم</p>
        <div
          class="msg"
          v-for="(message, index) in messages"
          :key="index"
          :class="message.type"
        >
          <p>
            {{ message.text }} <br />
            <span>{{ message.time }}</span>
          </p>
        </div>
      </div>

      <MessageInput @send-message="receiveMessage" />
    </div>
  </div>
</template>

<script>
import MessageInput from "@/components/MessageInput.vue";

export default {
  components: {
    MessageInput,
  },
  name: "SidebarRight",
  props: {
    selectedChat: Object,
  },
  data() {
    return {
      newMessage: "",
      localSelectedChat: { ...this.selectedChat },
    };
  },
  watch: {
    selectedChat: {
      handler(newVal) {
        this.localSelectedChat = { ...newVal };
      },
      immediate: true,
    },
  },
  computed: {
    messages() {
      return this.localSelectedChat?.messages || [];
    },
  },
  methods: {
    receiveMessage(message) {
      const newMessage = {
        type: "msg-me",
        text: message,
        time: new Date().toLocaleTimeString([], {
          hour: "2-digit",
          minute: "2-digit",
        }),
      };

      this.localSelectedChat.messages.push(newMessage);
    },

    setActiveChat() {
      let chats = document.querySelectorAll(".chat");
      chats.forEach((item) => {
        item.addEventListener("click", () => {
          chats.forEach((e) => {
            e.classList.remove("active");
          });
          item.classList.add("active");
          let numElement = item.querySelector(".num.unread");
          if (numElement) {
            numElement.textContent = "";
            numElement.classList.remove("unread");
          }
        });
      });
    },
  },
};
</script>

<style scoped>
.col-md-8 {
  padding-left: 0;
  padding-right: 0;
}
.right-side {
  background-color: #efddd5;
  height: 95vh;
}
.right-side .header {
  background-color: #d3d1d1;
  height: 10vh;
  padding: 10px 10px;
  /* margin-top: 9px; */
}

.right-side .header .user-details {
  display: flex;
  justify-content: start;
  align-items: start;
  gap: 10px;
}

.right-side .header .user-details .user-imgBx {
  width: 60px;
  height: 60px;
}

.right-side .header .user-details .user-imgBx {
  width: 60px;
  height: 60px;
}

.right-side .header .user-details h5 span {
  font-size: 0.8rem;
  color: #555;
}

/* chat box */

.right-side .chatBx {
  position: relative;
  width: 100%;
  height: 79vh;
  padding: 50px;
  overflow-y: auto;
  background-color: #efddd5;
  background-image: url("../../public/img/pattern.png");
  background-size: contain;
  background-position: center;
}

.right-side .chatBx p.date {
  font-size: 18px;
  position: sticky;
  top: -10%;
  right: 50%;
  text-align: center;
  z-index: 999;
  background-color: #817d7d;
  width: fit-content;
  color: #fff;
  padding: 3px;
  border-radius: 10px;
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  -ms-border-radius: 10px;
  -o-border-radius: 10px;
}

.right-side .chatBx .msg {
  position: relative;
  display: flex;
  width: 100%;
  margin: 5px 0;
}

.right-side .chatBx .msg p {
  position: relative;
  right: 0;
  text-align: right;
  max-width: 65%;
  padding: 12px;
  background: #dcf8c6;
  border-radius: 10px;
  font-size: 0.9rem;
}

.right-side .chatBx .msg-me p::before {
  content: "";
  position: absolute;
  top: 0;
  left: -10px;
  border-top: 10px solid #dcf8c6;
  border-left: 10px solid transparent;
  border-bottom: 10px solid transparent;
  border-right: 10px solid #dcf8c6;
}

.right-side .chatBx .msg p span {
  display: block;
  margin-top: 5px;
  font-size: 0.85rem;
  opacity: 0.5;
}

.right-side .chatBx .msg-me {
  justify-content: flex-end;
}

.right-side .chatBx .msg-frnd {
  justify-content: flex-start;
}

.right-side .chatBx .msg-frnd p {
  background: #fff;
  text-align: left;
}

.right-side .chatBx .msg-frnd p::before {
  content: "";
  position: absolute;
  top: 0;
  right: unset;
  right: -10px;
  border-top: 10px solid #fff;
  border-left: 10px solid #fff;
  border-bottom: 10px solid transparent;
  border-right: 10px solid transparent;
}
</style>
