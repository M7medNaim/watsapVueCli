<template>
  <div class="col-12 col-md-4 chatResponsive">
    <div class="left-side">
      <div class="inputSearch w-100 mb-2 mt-4 text-center">
        <input type="text" placeholder="... Search Chat" />
        <i class="fa-solid fa-magnifying-glass searchIcon"></i>
      </div>
      <div class="chat-list">
        <div
          class="chat d-flex justify-content-end align-items-center"
          v-for="(chat, index) in chats"
          :key="index"
          @click="openChat(chat, index)"
          :class="{ active: chat.isActive }"
        >
          <div class="imgBx">
            <img :src="chat.img" alt="" class="rounded-circle" />
          </div>
          <div class="details">
            <div class="head pe-2">
              <h4 class="name">{{ chat.name }}</h4>
              <span class="time">{{ chat.time }}</span>
            </div>
            <div class="msgs d-flex justify-content-between align-items-center">
              <p class="msg pe-2">{{ chat.message }}</p>
              <b
                class="num"
                :class="{ unread: chat.unread }"
                v-if="chat.unread && chat.unreadCount > 0"
              >
                {{ chat.unreadCount }}
              </b>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SidebarLeft",
  data() {
    return {
      chats: [
        {
          img: require("@/assets/img/img6.jpg"),
          name: "Ahmed Samy",
          time: "09:25",
          message: "مساء الخير",
          unread: true,
          unreadCount: 3,
          isActive: false,
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
        },
        {
          img: require("@/assets/img/img1.jpg"),
          name: "Ibrahem Ali",
          time: "10:00",
          message: "كيف حالك؟",
          unread: false,
          unreadCount: 0,
          isActive: false,
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
        },
      ],
    };
  },
  methods: {
    openChat(chat, index) {
      this.chats.forEach((item) => {
        item.isActive = false;
      });
      this.chats[index].isActive = true;

      this.chats[index].unread = false;
      this.chats[index].unreadCount = 0;
      this.$emit("select-chat", chat);
    },
  },
};
</script>

<style scoped>
.col-md-4 {
  padding-left: 0;
  padding-right: 0;
}
.left-side {
  background-color: #d3d1d1;
  height: 95vh;
  padding-top: 10px;
}
.left-side .inputSearch {
  position: relative;
}
.left-side .inputSearch .searchIcon {
  position: absolute;
  right: 7%;
  top: 30%;
  font-size: 18px;
  color: #737070;
}
.left-side .inputSearch input {
  outline: none;
  border: 1px solid #ffffff;
  padding: 10px 35px 10px 10px;
  background-color: #f4f3f3;
  width: 90%;
  height: 40px;
  border-radius: 10px;
  color: #737070;
  transition: all 0.3s;
}
.left-side .inputSearch input::placeholder {
  color: #585656ab;
  font-size: 14px;
}
.left-side .inputSearch input:focus {
  border: 1px solid #a28484ab;
}

/* left sidebar  */
.left-side .chat-list {
  position: relative;
  overflow-y: auto;
  height: 100%;
}
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

.left-side .chat-list .chat {
  position: relative;
  width: 100%;
  padding: 15px 15px 10px 15px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  cursor: pointer;
}

.left-side .chat-list .chat.active {
  background: #ebebeb;
}

.left-side .chat-list .chat:hover {
  background: #f5f5f5;
}

.left-side .chat-list .chat .imgBx {
  position: relative;
  width: 58px;
  height: 50px;
  overflow: hidden;
  border-radius: 50%;
  margin-right: 10px;
}

.left-side .chat-list .chat .imgBx img {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  object-fit: cover;
}

.left-side .chat-list .chat .details {
  position: relative;
  width: 100%;
}

.left-side .chat-list .chat .details .head {
  display: flex;
  justify-content: space-between;
}

.left-side .chat-list .chat .details .head .name {
  font-size: 1rem;
  font-weight: 600;
  color: #111;
  padding-top: 5px;
}

.left-side .chat-list .chat .details .head .time {
  font-size: 0.75rem;
  color: #aaa;
}

.left-side .chat-list .chat.active .details .head .time {
  color: #111;
}

.left-side .chat-list .chat .details .msg {
  color: #aaa;
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
