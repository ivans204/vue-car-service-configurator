<template>
  <nav class="d-flex">
    <div class="m-5">
      <img :src="require('@/assets/logo.png')" width="200" alt="Page logo">
    </div>
    <div class="m-5">
      <h3>Konfigurator servisa</h3>
      <p>Izračunajte trošak servisa</p>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Navbar"
}
</script>

<style scoped lang="scss">
.d-flex {
  display: flex;
}

.m-5 {
  margin: 5px;
}
</style>