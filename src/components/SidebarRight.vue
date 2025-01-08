<template>
  <div class="col-0 col-md-8 boxChat px-0" v-if="selectedChat">
    <div class="right-side" style="background-color: #efddd5; height: 95vh">
      <div
        class="header position-relative bg-secondary-subtle px-2 border-1 border-end border-secondary-subtle"
      >
        <div class="row h-100">
          <div class="col-4 h-100">
            <div
              class="user-details d-flex justify-content-start align-items-center gap-2 h-100"
            >
              <div class="user-imgBx">
                <img
                  :src="selectedChat.img"
                  alt=""
                  class="img-fluid rounded-circle w-100 h-100"
                />
              </div>
              <h6 class="my-auto">{{ selectedChat.name }} <br /></h6>
            </div>
          </div>
          <div
            class="col-8 search_list d-flex justify-content-end align-items-center gap-3 fs-5 h-100"
          >
            <div id="bxSearch" class="d-flex">
              <div
                class="searchInput align-items-center"
                :class="{ 'show-search': isSearchBarVisible }"
              >
                <input
                  type="text"
                  name="searchChat"
                  id="searchChat"
                  class="ms-3 border-0 rounded-1 py-1 px-2 fs-6 w-100 ms-5"
                  v-model="searchQuery"
                  placeholder="... Search Here"
                  style="outline: none"
                />
              </div>
              <button
                @click="toggleSearchInput()"
                aria-label="search in chat"
                class="border-0 bg-transparent"
              >
                <i
                  class="fa-solid fa-magnifying-glass searchIcon me-2 text-secondary cursor-pointer"
                ></i>
              </button>
            </div>
            <button
              @click.stop="showList()"
              aria-label="list chat (menu)"
              class="border-0 bg-transparent"
            >
              <i
                class="fa-solid fa-ellipsis-vertical text-secondary"
                style="color: #5a5757"
              ></i>
            </button>
          </div>
        </div>
      </div>

      <div
        class="chatBx position-relative w-100 overflow-auto px-5 pt-5 pb-3"
        ref="chatBox"
      >
        <div
          ref="menu"
          v-click-outside="closeMenu"
          class="menu align-items-center text-end bg-secondary-subtle rounded-2 lh-base position-fixed z-3"
          :class="{ 'show-list': showListVisible }"
        >
          <ul class="list-unstyled p-0">
            <li><a href="#" @click="handleArchiveChat">Archive Chat</a></li>
            <li><a href="#" @click="handlePinChat">Pin Chat</a></li>
            <li>
              <a href="#" @click="handleLabelClick">Label Chat</a>
            </li>
            <li>
              <a href="#" @click="$emit('mark-as-unread', chat)"
                >Mark as Unread</a
              >
            </li>
            <li><a href="#" @click="handleDeleteChat">Delete Chat</a></li>
          </ul>
        </div>
        <p
          class="date fs-6 text-center bg-secondary text-white py-1 px-3 rounded-3 position-sticky end-50 z-3"
        >
          اليوم
        </p>
        <div
          class="msg position-relative w-100 d-flex my-1"
          v-for="(message, index) in filteredMessages"
          :key="index"
          :class="message.type"
        >
          <p
            class="position-relative text-start px-3 py-2 start-0 rounded-2 fst-normal text-break text-wrap lh-base"
          >
            {{ message.text }} <br />
            <span class="d-block mt-1 opacity-50 fst-normal">{{
              message.time
            }}</span>
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
  emits: ["mark-as-unread", "pin-chat", "open-label", "delete-chat"],
  components: {
    MessageInput,
  },
  name: "SidebarRight",
  props: {
    selectedChat: Object,
  },
  data() {
    return {
      isSearchBarVisible: false,
      showListVisible: false,
      newMessage: "",
      searchQuery: "",
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
    filteredMessages() {
      if (!this.searchQuery) {
        return this.messages;
      }

      return this.messages.filter((message) => {
        return message.text
          .toLowerCase()
          .includes(this.searchQuery.toLowerCase());
      });
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
      this.scrollToBottom();
    },
    scrollToBottom() {
      this.$nextTick(() => {
        const chatMessages = this.$refs.chatBox.querySelectorAll(".msg");
        const lastMessage = chatMessages[chatMessages.length - 1];
        if (lastMessage) {
          lastMessage.scrollIntoView({ behavior: "smooth", block: "end" });
        }
      });
    },

    toggleSearchInput() {
      this.isSearchBarVisible = !this.isSearchBarVisible;
    },
    showList() {
      this.showListVisible = !this.showListVisible;
    },
    closeMenu() {
      this.showListVisible = false;
    },

    setActiveChat(index) {
      this.activeChat = index;
      const chat = this.chats[index];
      if (chat.unread) {
        chat.unread = false;
        chat.unreadCount = 0;
      }
    },
    handlePinChat() {
      this.$emit("pin-chat");
    },
    handleLabelClick() {
      this.$emit("open-label");
    },
    handleDeleteChat() {
      this.$emit("delete-chat");
    },
  },
  directives: {
    clickOutside: {
      beforeMount(el, binding) {
        el.clickOutsideEvent = (event) => {
          if (!(el === event.target || el.contains(event.target))) {
            binding.value(event);
          }
        };
        document.body.addEventListener("click", el.clickOutsideEvent);
      },
      unmounted(el) {
        document.body.removeEventListener("click", el.clickOutsideEvent);
      },
    },
  },
};
</script>

<style scoped>
.right-side .header {
  height: 8vh;
}

.right-side .header .user-details .user-imgBx {
  width: 45px;
  height: 45px;
}

.searchInput {
  opacity: 0;
  visibility: hidden;
  transition: all 0.5s;
}
.searchInput.show-search {
  opacity: 1;
  visibility: visible;
}

/* chat box */

.right-side .chatBx {
  height: calc(87vh - 60px);
  background-image: url("../../public/img/pattern.png");
  background-size: contain;
  background-position: center;
}

.right-side .chatBx .menu {
  top: 12.5%;
  left: 1%;
  opacity: 0;
  visibility: hidden;
  transition: all 0.5s;
}

.right-side .chatBx .menu.show-list {
  opacity: 1;
  visibility: visible;
}
.right-side .chatBx .menu ul li {
  padding: 10px 10px 10px 20px;
  transition: all 0.2s;
}
.right-side .chatBx .menu ul li:hover {
  background-color: #fff;
}

.right-side .chatBx p.date {
  top: -10%;
  width: fit-content;
}

.right-side .chatBx .msg p {
  max-width: 65%;
  background: #dcf8c6;
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
/* scroll style */
.right-side .chatBx::-webkit-scrollbar {
  width: 10px;
}

.right-side .chatBx::-webkit-scrollbar-track {
  background: #edebeb97;
}

.right-side .chatBx::-webkit-scrollbar-thumb {
  background: #6d6c6ca7;
  border-radius: 8px;
  -webkit-border-radius: 8px;
  -moz-border-radius: 8px;
  -ms-border-radius: 8px;
  -o-border-radius: 8px;
}
</style>
