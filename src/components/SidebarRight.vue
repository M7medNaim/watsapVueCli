<template>
  <div class="col-0 col-md-8 boxChat" v-if="selectedChat">
    <div class="right-side">
      <div class="header">
        <div class="row">
          <div class="col-6">
            <div
              class="user-details d-flex justify-content-start align-items-center"
            >
              <div class="user-imgBx">
                <img
                  :src="selectedChat.img"
                  alt=""
                  class="img-fluid rounded-circle"
                />
              </div>
              <h6 class="pt-1">{{ selectedChat.name }} <br /></h6>
            </div>
          </div>
          <div class="col-6">
            <div
              class="search_list d-flex justify-content-end align-items-center gap-3 fs-5"
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
                    class="ms-3"
                    placeholder="... Search Here"
                  />
                </div>
                <button @click="toggleSearchInput()">
                  <i class="fa-solid fa-magnifying-glass searchIcon"></i>
                </button>
              </div>
              <button @click="showList()">
                <i
                  class="fa-solid fa-ellipsis-vertical"
                  style="color: #5a5757"
                ></i>
              </button>
            </div>
          </div>
        </div>
      </div>

      <div class="chatBx">
        <div
          class="menu align-items-center"
          :class="{ 'show-list': showListVisible }"
        >
          <ul>
            <li><a href="#">Archive Chat</a></li>
            <li><a href="#">Pin Chat</a></li>
            <li><a href="#">Label Chat</a></li>
            <li><a href="#">Mark as Unread</a></li>
            <li><a href="#">Block, Delete Chat</a></li>
          </ul>
        </div>
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
      isSearchBarVisible: false,
      showListVisible: false,
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
    toggleSearchInput() {
      this.isSearchBarVisible = !this.isSearchBarVisible;
    },
    showList() {
      this.showListVisible = !this.showListVisible;
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
  position: relative;
  background-color: #d3d1d1;
  height: 8vh;
  padding: 5px 10px;
  border-right: 1px solid #b8b6b6;
}

.right-side .header .user-details {
  gap: 10px;
  height: 8vh;
}

.right-side .header .user-details .user-imgBx {
  width: 45px;
  height: 45px;
}

.right-side .header .user-details .user-imgBx img {
  width: 45px;
  height: 45px;
}
.search_list {
  height: 8vh;
}
.search_list #bxSearch input {
  box-sizing: border-box;
  border: none;
  border-radius: 5px;
  outline: none;
  padding: 5px 10px;
  font-size: 14px;
  width: 350px;
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
.search_list #bxSearch .searchIcon {
  cursor: pointer;
  color: #5a5757;
}

/* chat box */

.right-side .chatBx {
  position: relative;
  width: 100%;
  height: calc(87vh - 60px);
  padding: 50px;
  overflow-y: auto;
  background-color: #efddd5;
  background-image: url("../../public/img/pattern.png");
  background-size: contain;
  background-position: center;
}

.right-side .chatBx .menu {
  position: fixed;
  top: 12.5%;
  left: 1%;
  z-index: 999;
  background-color: #eee;
  border-radius: 10px;
  text-align: end;
  line-height: 30px;
  opacity: 0;
  visibility: hidden;
  transition: all 0.5s;
}

.right-side .chatBx .menu.show-list {
  opacity: 1;
  visibility: visible;
}

.right-side .chatBx .menu ul {
  padding-left: 0;
}
.right-side .chatBx .menu ul li {
  padding: 10px 10px 10px 20px;
  transition: all 0.2s;
}
.right-side .chatBx .menu ul li:hover {
  background-color: #fff;
}
.right-side .chatBx .menu ul li a {
  text-decoration: none;
  color: #000;
}

.right-side .chatBx p.date {
  font-size: 18px;
  position: sticky;
  top: -10%;
  right: 50%;
  text-align: center;
  z-index: 999;
  background-color: #787676;
  width: fit-content;
  color: #fff;
  padding: 3px 12px;
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
  font-size: 0.8rem;
  word-wrap: break-word;
  overflow-wrap: break-word;
  white-space: normal;
  line-height: 1.5;
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
