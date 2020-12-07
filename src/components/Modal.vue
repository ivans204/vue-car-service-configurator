<template>
  <div class="modal">

    <div class="modal-box">
      <h2 class="modal-title">Konfigurator servisa</h2>
      <div @click="closeModal" class="modal-exit">X</div>
      <slot></slot>

    </div>

  </div>
</template>

<script>
export default {
  name: "Modal",
  data: () => ({
    modalClose: true
  }),
  methods: {
    closeModal() {
      this.$emit('closeModal')
    },
  }
}
</script>

<style scoped lang="scss">
.modal {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background-color: rgba(0, 0, 0, .5);
  height: 100%;
  width: 100%;
  z-index: 10;
}

.modal-box {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 900px;
  background-color: white;
  border: 2px solid black;
  padding: 20px;

  .modal-exit {
    position: absolute;
    top: 20px;
    right: 20px;
    cursor: pointer;
  }

  .modal-title {
    text-align: center;
    margin: 0;
  }
}
</style>