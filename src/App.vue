<template>
  <main class="overflow-hidden h-100 w-100">
    <div id="app vh-100">
      <!-- <HeaderComponent /> -->
      <div class="app">
        <div class="row mx-0">
          <SidebarLeft @select-chat="setSelectedChat" ref="leftSidebar" />
          <SidebarRight
            :selectedChat="selectedChat"
            v-if="selectedChat"
            @mark-as-unread="handleMarkAsUnread"
            @pin-chat="pinActiveChat"
            :chats="chats"
            @open-label="handleOpenLabel"
            @delete-chat="handleDeleteChat"
          />
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import SidebarLeft from "./components/SidebarLeft.vue";
import SidebarRight from "./components/SidebarRight.vue";

export default {
  name: "App",
  components: {
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
    pinActiveChat() {
      const activeChat = this.$refs.leftSidebar.chats.find(
        (chat) => chat.isActive
      );
      if (activeChat) {
        this.$refs.leftSidebar.pinChat(activeChat);
      }
    },
    handleOpenLabel() {
      this.$refs.leftSidebar.openLabelModal();
    },
    handleDeleteChat() {
      this.$refs.leftSidebar.deleteChat();
      this.selectedChat = null;
    },
  },
};
</script>

<style>
/* google fonts */
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;500;600;700&display=swap");

/* global style  */
a {
  text-decoration: none !important;
  color: #000 !important;
}
</style>
