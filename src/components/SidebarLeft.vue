<template>
  <div class="col-12 col-md-4 chatResponsive px-0">
    <div class="left-side pt-2 bg-secondary-subtle">
      <div class="inputSearch w-100 mb-2 mt-4 text-center position-relative">
        <input
          type="text"
          placeholder="... Search Chat"
          v-model="searchQuery"
          class="border border-1 border-white py-2 pe-5 ps-2 rounded-2 bg-body text-secondary"
        />
        <i
          class="fa-solid fa-magnifying-glass searchIcon fs-5 text-secondary"
        ></i>
      </div>
      <div class="chat-list position-relative overflow-auto h-100">
        <div
          class="chat d-flex justify-content-end align-items-center position-relative w-100 px-3 pt-2 pb-3 border-1 border-bottom border-secondary-subtle"
          v-for="(chat, index) in filteredChats"
          :key="index"
          @click="openChat(chat, index)"
          :class="{ active: chat.isActive }"
        >
          <div class="imgBx position-relative overflow-hidden h-25 me-2">
            <img :src="chat.img" alt="" class="rounded-circle w-100 h-100" />
          </div>
          <div class="details position-relative w-100">
            <div class="head pe-2 d-flex justify-content-between">
              <h4 class="name fs-6 fw-medium text-black pt-2">
                {{ chat.name }}
              </h4>
              <span class="time text-secondary fst-normal">{{
                chat.time
              }}</span>
            </div>
            <div
              class="msgs d-flex justify-content-between align-items-center text-secondary"
            >
              <p class="msg pe-2">{{ chat.message }}</p>
              <div class="d-flex align-items-center gap-3">
                <b
                  class="num"
                  :class="{ unread: chat.unread }"
                  v-if="chat.unread"
                >
                  {{ chat.unreadCount }}
                </b>
                <i
                  v-if="chat.pinned"
                  class="fa-solid fa-thumbtack pin-icon"
                  title="Pinned Chat"
                ></i>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div ref="labelModal" class="modal fade" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="row pt-2">
              <div class="col-6">
                <h5 class="modal-title text-end">Label Chat</h5>
              </div>
              <div class="col-6 text-start">
                <button
                  type="button"
                  class="btn-close"
                  @click="hideBootstrapModal"
                ></button>
              </div>
            </div>
            <div class="modal-body">
              <input v-model="newLabel" type="text" class="form-control" />
            </div>
            <div class="modal-footer">
              <button
                type="button"
                class="btn btn-secondary"
                @click="hideBootstrapModal"
              >
                Close
              </button>
              <button type="button" class="btn btn-primary" @click="setLabel">
                Save
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Modal } from "bootstrap";
export default {
  name: "SidebarLeft",
  data() {
    return {
      searchQuery: "",
      chats: [
        {
          img: require("@/assets/img/img6.jpg"),
          name: "Ahmed Samy",
          time: "09:25",
          message: "مساء الخير",
          unread: true,
          unreadCount: 3,
          isActive: false,
          pinned: false,
          label: "",
          messages: [
            { type: "msg-me", text: "مرحباً!", time: "12:15" },
            { type: "msg-frnd", text: "أهلاً بك", time: "12:16" },
            { type: "msg-me", text: "مرحباً!", time: "12:15" },
            { type: "msg-frnd", text: "أهلاً بك", time: "12:16" },
            { type: "msg-me", text: "مرحباً!", time: "12:15" },
            { type: "msg-frnd", text: "أهلاً بك", time: "12:16" },
            { type: "msg-me", text: "مرحباً!", time: "12:15" },
            { type: "msg-frnd", text: "أهلاً بك", time: "12:16" },
            { type: "msg-me", text: "مرحباً!", time: "12:15" },
            { type: "msg-frnd", text: "أهلاً بك", time: "12:16" },
            { type: "msg-me", text: "مرحباً!", time: "12:15" },
            { type: "msg-frnd", text: "أهلاً بك", time: "12:16" },
            { type: "msg-me", text: "مرحباً!", time: "12:15" },
            { type: "msg-frnd", text: "أهلاً بك", time: "12:16" },
          ],
          selectedChat: null,
          newLabel: "",
          modalInstance: null,
        },
        {
          img: require("@/assets/img/img1.jpg"),
          name: "Ibrahem Ali",
          time: "10:00",
          message: "كيف حالك؟",
          unread: false,
          unreadCount: 0,
          isActive: false,
          pinned: false,
          label: "",
          messages: [
            { type: "msg-me", text: "كيف حالك؟", time: "10:30" },
            { type: "msg-frnd", text: "أنا بخير", time: "10:31" },
            { type: "msg-me", text: "كيف حالك؟", time: "10:30" },
            { type: "msg-frnd", text: "أنا بخير", time: "10:31" },
            { type: "msg-me", text: "كيف حالك؟", time: "10:30" },
            { type: "msg-frnd", text: "أنا بخير", time: "10:31" },
            { type: "msg-me", text: "كيف حالك؟", time: "10:30" },
            { type: "msg-frnd", text: "أنا بخير", time: "10:31" },
            { type: "msg-me", text: "كيف حالك؟", time: "10:30" },
            { type: "msg-frnd", text: "أنا بخير", time: "10:31" },
          ],
          selectedChat: null,
          newLabel: "",
          modalInstance: null,
        },
      ],
    };
  },
  computed: {
    filteredChats() {
      if (!this.searchQuery) {
        return this.chats.slice().sort((a, b) => {
          if (a.pinned && !b.pinned) return -1;
          if (!a.pinned && b.pinned) return 1;
          return 0;
        });
      }

      return this.chats
        .filter((chat) => {
          const lowerQuery = this.searchQuery.toLowerCase();
          return chat.name.toLowerCase().includes(lowerQuery);
          // chat.message.toLowerCase().includes(lowerQuery)
        })
        .sort((a, b) => {
          if (a.pinned && !b.pinned) return -1;
          if (!a.pinned && b.pinned) return 1;
          return 0;
        });
    },
  },
  methods: {
    openChat(chat, index) {
      if (index >= 0 && index < this.chats.length) {
        this.chats.forEach((item) => {
          item.isActive = false;
        });

        this.chats[index].isActive = true;

        this.selectedChat = this.chats[index];
        this.newLabel = this.selectedChat.name;

        this.chats[index].unread = false;
        this.chats[index].unreadCount = 0;

        this.$emit("select-chat", chat);
      } else {
        console.error("The chat does not exist");
      }
    },
    markAsUnread(chat) {
      chat.unread = true;
      chat.unreadCount = "";
    },
    pinChat(chat) {
      const activeChat = this.chats.find((c) => c.isActive);

      chat.pinned = !chat.pinned;

      this.chats = this.chats.slice().sort((a, b) => {
        if (a.pinned && !b.pinned) return -1;
        if (!a.pinned && b.pinned) return 1;
        return 0;
      });

      if (!chat.pinned) {
        const pinnedChat = this.chats.find((c) => c.pinned);
        if (pinnedChat) {
          this.chats.forEach((c) => (c.isActive = false));
          pinnedChat.isActive = true;
        } else {
          if (activeChat) {
            const activeIndex = this.chats.findIndex(
              (c) => c.id === activeChat.id
            );
            if (activeIndex !== -1) {
              this.chats[activeIndex].isActive = true;
            }
          }
        }
      } else {
        this.chats.forEach((chat) => (chat.isActive = false));
        chat.isActive = true;
      }
    },
    openLabelModal() {
      if (this.selectedChat) {
        this.newLabel = this.selectedChat.name;
        this.showBootstrapModal();
      }
    },
    setLabel() {
      if (this.selectedChat) {
        this.selectedChat.name = this.newLabel;
        this.hideBootstrapModal();
      }
    },
    showBootstrapModal() {
      const modalElement = this.$refs.labelModal;
      if (modalElement) {
        const modal = new Modal(modalElement);
        modal.show();
      }
    },
    hideBootstrapModal() {
      const modalElement = this.$refs.labelModal;
      if (modalElement) {
        const modal = Modal.getInstance(modalElement);
        modal.hide();
      }
    },
    deleteChat() {
      const activeChat = this.chats.find((chat) => chat.isActive);
      if (activeChat) {
        const index = this.chats.indexOf(activeChat);
        if (index !== -1) {
          this.chats.forEach((chat, idx) => {
            this.chats[idx].isActive = false;
          });
          this.chats.splice(index, 1);
        }
      }
    },
  },
  mounted() {
    if (this.$refs.labelModal) {
      this.modalInstance = new Modal(this.$refs.labelModal);
    }
  },
};
</script>

<style scoped>
.left-side {
  height: 95vh;
}
.left-side .inputSearch .searchIcon {
  position: absolute;
  right: 8%;
  top: 30%;
}
.left-side .inputSearch input {
  outline: none;
  width: 90%;
  height: 40px;
  transition: all 0.3s;
}
.left-side .inputSearch input::placeholder {
  font-size: 14px;
}
.left-side .inputSearch input:focus {
  border: 1px solid #a28484ab;
}

/* left sidebar  */
/* scroll style */
.left-side .chat-list::-webkit-scrollbar {
  width: 10px;
}

.left-side .chat-list::-webkit-scrollbar-track {
  background: #edebeb97;
}

.left-side .chat-list::-webkit-scrollbar-thumb {
  background: #6d6c6ca7;
  border-radius: 8px;
  -webkit-border-radius: 8px;
  -moz-border-radius: 8px;
  -ms-border-radius: 8px;
  -o-border-radius: 8px;
}
/* end scroll style */

.left-side .chat-list .chat {
  cursor: pointer;
}

.left-side .chat-list .chat.active {
  background: #ebebeb;
}

.left-side .chat-list .chat:hover {
  background: #f5f5f5;
}

.left-side .chat-list .chat .imgBx {
  width: 70px;
}

.left-side .chat-list .chat .details .msg {
  display: -webkit-box;
  -webkit-line-clamp: 1;
  font-size: 0.9rem;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}

.left-side .chat-list .chat .details .num.unread {
  background-color: #06d755;
  color: #fff;
  min-width: 20px;
  height: 20px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 0.75rem;
}
</style>
