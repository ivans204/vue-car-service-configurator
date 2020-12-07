<template>
  <div>
    <p class="configurator-step ">Korak 3: Vaši osobni podaci</p>

    <div v-if="errors.length" class="error-msgs">
      <p>Molim ispravite greške: </p>
      <p class="text-danger" v-for="error in errors" :key="error">{{ error }}</p>
    </div>

    <div class="contact">
      <input class="contact-item" type="text" placeholder="Ime i prezime*" v-model="contactDetails.name" required>
      <input class="contact-item" type="email" placeholder="Email adresa*" v-model="contactDetails.email" required>
      <input class="contact-item" type="number" placeholder="Broj telefona*" v-model="contactDetails.number" required>
      <textarea class="contact-item" cols="30" rows="8" placeholder="Napomena (opcinalno)"
                v-model="contactDetails.msg"></textarea>
    </div>

    <div class="controlButtons">
      <button @click="prevStep">Nazad</button>
      <button type="submit" @click="validateForm">Dalje</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "ContactForm",
  props: {
    configuratorData: Object
  },
  data: () => ({
    errors: [],
    contactDetails: {
      name: null,
      email: null,
      number: null,
      msg: null,
    }
  }),
  methods: {
    validateForm() {
      if (this.contactDetails.name && this.validateEmail(this.contactDetails.email) && this.contactDetails.number) {
        this.nextStep()
      }

      this.errors = []

      if (!this.contactDetails.name) this.errors.push('Potrebno je unijeti ime i prezime.')

      if (!this.contactDetails.email) {
        this.errors.push('Potrebno je unijeti email.')
      } else if (!this.validateEmail(this.contactDetails.email)) {
        this.errors.push('Potrebno je unijeti ispravan email.')
      }

      if (!this.contactDetails.number) {
        this.errors.push('Potrebno je unijeti broj telefona.')
      } else if (this.contactDetails.number.length < 9) {
        this.errors.push('Potrebno je unijeti ispravan broj telefona.')
      }

    },
    validateEmail: function (email) {
      const reg = /^(([^<>()\]\\.,;:\s@"]+(\.[^<>()\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,24}))$/;
      return reg.test(email);
    },
    prevStep() {
      this.$emit('changeComponent', 'CarService')
    },
    nextStep() {
      this.$emit('saveContactDetails', this.contactDetails)
      this.$emit('changeComponent', 'DetailsReview')
    },
  },
  mounted() {
    this.configuratorData.contactDetails ? this.contactDetails = this.configuratorData.contactDetails : this.contactDetails = {}
  }
}
</script>

<style scoped lang="scss">
.configurator-step {
  margin: 30px 0;
  font-size: 1.2rem;
}

.contact {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 50px;

  .contact-item {
    flex: 0 1 45%;
    height: 100%;
    margin: 10px;
    padding: 5px;
    border: 2px solid black;
  }
}

.text-danger {
  color: #dc3545;
}

.controlButtons {
  text-align: right;
  border-top: 1px solid black;

  button {
    width: 100px;
    height: 30px;
    margin-top: 20px;
    margin-left: 20px;
  }
}
</style>