<template>
  <div>
    <h2>Vaša prijava je uspješno poslana</h2>
    <p>
      Vaša prijava je uspješno poslana i zaprimljena.
      Kontaktirati ćemo vas u najkraćem mogućem roku.
      Hvala vam.
    </p>
    <button @click="closeModal">Zatvori</button>
  </div>
</template>

<script>
export default {
  name: "ConfigSent",
  methods: {
    closeModal() {
      this.$emit('closeModal')
    }
  }
}
</script>

<style scoped lang="scss">

h2 {
  margin-top: 50px;
}

h2, p {
  text-align: center;
}

button {
  display: block;
  margin: 50px auto;
}
</style>