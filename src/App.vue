<template>
  <main>
    <div id="app">
      <HeaderComponent />
      <div class="app">
        <div class="row">
          <SidebarLeft @select-chat="setSelectedChat" ref="leftSidebar" />
          <SidebarRight
            :selectedChat="selectedChat"
            v-if="selectedChat"
            @mark-as-unread="handleMarkAsUnread"
            @pin-chat="pinActiveChatInLeftSidebar"
          />
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import HeaderComponent from "./components/HeaderComponent.vue";
import SidebarLeft from "./components/SidebarLeft.vue";
import SidebarRight from "./components/SidebarRight.vue";

export default {
  name: "App",
  components: {
    HeaderComponent,
    SidebarLeft,
    SidebarRight,
  },
  data() {
    return {
      selectedChat: null,
    };
  },
  methods: {
    setSelectedChat(chat) {
      this.selectedChat = chat;
    },
    handleMarkAsUnread() {
      const activeChat = this.$refs.leftSidebar.chats.find(
        (chat) => chat.isActive
      );
      if (activeChat) {
        this.$refs.leftSidebar.markAsUnread(activeChat);
      }
    },
    pinActiveChatInLeftSidebar() {
      const activeChat = this.$refs.leftSidebar.chats.find(
        (chat) => chat.isActive
      );
      if (activeChat) {
        this.$refs.leftSidebar.pinChat(activeChat);
      }
    },
  },
};
</script>

<style>
/* google fonts */
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;500;600;700&display=swap");

/* global style  */
* {
  font-family: "Open Sans", sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

ul {
  list-style: none;
}

body {
  height: 100vh;
  background-image: url("../public/img/pattern.png");
  background-size: contain;
  background-position: center;
  background-color: #efddd5 !important;
}

button {
  background-color: transparent;
  border: none;
}
main {
  overflow: hidden;
}
main .app {
  height: 95vh;
}

main .app .row {
  margin-left: 0;
  margin-right: 0;
}

.col-md-8,
.col-md-4 {
  padding-left: 0;
  padding-right: 0;
}
</style>
