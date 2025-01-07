<template>
  <header class="w-100 bg-white">
    <div class="row">
      <div class="col-6 close">
        <button aria-label="close button">
          <i class="fa-solid fa-x pe-4 pt-2 text-secondary fs-6"></i>
        </button>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: "HeaderComponent",
};
</script>

<style scoped>
header {
  height: 5vh;
}
</style>
