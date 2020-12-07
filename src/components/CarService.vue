<template>
  <div>

    <p class="configurator-step ">Korak 2: Odaberite jednu ili više usluga</p>

    <div class="car-service-wrap">
      <span class="car-service-input" v-for="service in carServices" :key="service.name">
        <input type="checkbox" :value="service" :id="service.name" v-model="selectedServices"
               :checked="selectedServices">
        <label :for="service.name">{{ service.name }} ({{ service.price }} kn)</label>
      </span>
    </div>

    <div class="car-service-checkout">

      <p :class="{'text-success': discountIsActive, 'text-danger': !discountIsActive}">{{ discountMsg }}</p>
      <div class="discount-wrap" v-if="!discountIsActive">
        <input class="checkout-discount" type="text" v-model="enteredCode" placeholder="Unesite kod kupona ovdje">
        <button class="checkout-discount" @click="checkDiscount" :disabled="!enteredCode">Primjeni</button>
      </div>

      <div v-if="discountIsActive">
        <p class="uppercase">Osnovica: <strong> {{ basePrice }} kn</strong></p>
        <p>Popust ({{ discount.value * 100 }}%): <strong class="uppercase">-{{ basePrice * discount.value }} kn</strong>
        </p>
      </div>

      <p class="checkout-price uppercase">Ukupno:
        <strong>{{ discountIsActive ? discountPrice : basePrice }} kn</strong>
      </p>
    </div>

    <div class="controlButtons">
      <button @click="prevStep">Nazad</button>
      <button @click="nextStep" :disabled="!selectedServices.length">Dalje</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "CarService",
  props: {
    configuratorData: Object
  },
  data: () => ({
    carServices: [
      {name: 'Zamjena ulja i filtera', price: 500},
      {name: 'Promjena pakni', price: 450},
      {name: 'Promjena guma', price: 100},
      {name: 'Servis klima uređaja', price: 299},
      {name: 'Balansiranje guma', price: 50},
      {name: 'Zamjena ulja u kočnicama', price: 229},
    ],
    selectedServices: [],
    priceData: {},
    enteredCode: '',
    discountMsg: '',
    discount: {
      code: 'Tokić123',
      value: 0.3
    },
    discountIsActive: false,
  }),
  methods: {
    checkDiscount() {
      if (this.enteredCode === this.discount.code) {
        this.discountMsg = 'Hvala vam, unijeli ste ispravan kod kupona.'
        this.discountIsActive = true
        this.priceData.discountValue = this.discount.value
      } else {
        this.discountMsg = 'Nažalost kod kupona nije ispravan.'
      }
    },
    saveServicePriceData() {
      this.priceData.basePrice = this.basePrice
      if (this.discountIsActive) this.priceData.discountPrice = this.discountPrice
    },
    nextStep() {
      this.saveServicePriceData()

      this.$emit('saveCarServiceData', this.selectedServices)
      this.$emit('saveServicePrice', this.priceData)
      this.$emit('changeComponent', 'ContactForm')
    },
    prevStep() {
      this.$emit('changeComponent', 'CarModel')
    },
  },
  computed: {
    basePrice() {
      return this.selectedServices.reduce((sum, item) => sum + item.price, 0)
    },
    discountPrice() {
      return this.basePrice * (1 - this.discount.value)
    }
  },
  mounted() {
    if (this.configuratorData.selectedServices) {
      this.selectedServices = this.configuratorData.selectedServices
    } else {
      this.selectedServices = []
    }

    if (this.configuratorData.priceData) {
      this.priceData = this.configuratorData.priceData
      this.priceData.discountValue ? this.discountIsActive = true : this.discountIsActive = false
    } else {
      this.priceData = {}
    }

  },
}
</script>

<style scoped lang="scss">

.configurator-step {
  margin: 30px 0;
  font-size: 1.2rem;
}

.car-service-wrap {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  margin-bottom: 125px;
}

.car-service-input {
  flex: 0 1 30%;
  margin-bottom: 15px;
}

.car-service-checkout {
  width: 100%;
  text-align: right;

  .text-success {
    color: #28a745;
  }

  .text-danger {
    color: #dc3545;
  }

  .checkout-discount {
    height: 30px;
    border-radius: 0;
    border: 1px solid black;
  }

  input.checkout-discount {
    padding-left: 10px;
  }

  .checkout-price {
    font-size: 1.5rem;
  }
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

.uppercase {
  text-transform: uppercase;
}
</style>