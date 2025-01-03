<template>
  <header>
    <div class="row">
      <div class="col-6 close">
        <button><i class="fa-solid fa-x pe-4 pt-2"></i></button>
      </div>
      <!-- <div class="col-6 imgWats ps-4 fs-5 text-start">
        <i class="fa-brands fa-whatsapp"></i>
      </div> -->
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
  width: 100%;
  height: 5vh;
  background-color: #fff;
}

header .row .imgWats {
  color: green;
}

header .row .close i {
  font-size: 15px;
  color: #aca8a8;
}
</style>
