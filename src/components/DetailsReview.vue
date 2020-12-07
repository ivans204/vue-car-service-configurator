<template>
  <div>
    <p class="configurator-step">Korak 4. Pregled i potvrda vašeg odabira</p>
    <p>
      Molimo vas da još jednom pregledate i potvrdite unesene podatke. Ukoliko želite pormijeniti neki od podataka,
      možete pritisnuti gumb za uređivanje pored svake od kategorija. Kada ste provjerili i potvrdili ispravnost svojih
      podataka pritisnite gumb pošalji na dnu, za slanje upita na servis.
    </p>


    <div class="details-container">

      <div class="category pr">
        <div class="category-title">
          <h2 class="uppercase">Model vozila</h2>
          <button class="edit-category" @click="changeComp('CarModel')">Uredi</button>
        </div>
        <p>{{ configuratorData.carModel }}</p>
      </div>

      <div class="category pl">
        <div class="category-title">
          <h2 class="uppercase">Odabrane usluge</h2>
          <button class="edit-category" @click="changeComp('CarService')">Uredi</button>
        </div>

        <ul class="services-list">
          <li class="service-item" v-for="service in configuratorData.selectedServices" :key="service.name">
            <span>{{ service.name }}</span>
            <span class="category-text uppercase">{{ withZeros(service.price) }} kn</span>
          </li>
        </ul>

        <p class="text-right" v-if="configuratorData.priceData.discountPrice">Popust
          ( {{ configuratorData.priceData.discountValue * 100 }}% ):
          <span class="uppercase">-{{
              withZeros(configuratorData.priceData.basePrice * configuratorData.priceData.discountValue)
            }} kn</span>
        </p>
        <p class="text-right uppercase">Ukupno: <strong>{{ showTotalPrice }} kn</strong></p>

      </div>
    </div>

    <div class="details-container">
      <div class="category-title w-100">
        <h2>Kontakt podaci</h2>
        <button class="edit-category" @click="changeComp('ContactForm')">Uredi</button>
      </div>
      <p class="category pr">Ime i prezime: <span class="category-text"> {{
          configuratorData.contactDetails.name
        }}</span>
      </p>
      <p class="category pl">Email adresa: <span class="category-text"> {{
          configuratorData.contactDetails.email
        }}</span>
      </p>
      <p class="category pr">Broj telefona: <span class="category-text">
        {{ configuratorData.contactDetails.number }}</span></p>
      <p class="category pl">Napomena: <span class="category-text"> {{ configuratorData.contactDetails.msg }}</span></p>
    </div>


    <div class="controlButtons">
      <button @click="prevStep">Nazad</button>
      <button @click="nextStep">Pošalji</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "DataReview",
  props: {
    configuratorData: Object,
  },
  methods: {
    changeComp(compName) {
      this.$emit('changeComponent', compName)
    },
    prevStep() {
      this.$emit('changeComponent', 'ContactForm')
    },
    nextStep() {
      this.$emit('changeComponent', 'ConfigSent')
    },
    withZeros(number) {
      return number.toLocaleString("en", {useGrouping: false, minimumFractionDigits: 2})
    }
  },
  computed: {
    showTotalPrice: function () {
      return this.configuratorData.priceData.discountValue
          ? this.withZeros(this.configuratorData.priceData.discountPrice)
          : this.withZeros(this.configuratorData.priceData.basePrice)
    }
  }
}
</script>

<style scoped lang="scss">
.configurator-step {
  margin: 30px 0;
  font-size: 1.2rem;
}

.category-title {
  display: flex;
  align-items: center;

  .edit-category {
    margin-left: 10px;
  }
}

.services-list {
  list-style: none;

  .service-item {
    margin-top: 1rem;
    margin-bottom: 1rem;
  }
}

.details-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;

  .category {
    flex: 0 1 50%;
  }
}

.uppercase {
  text-transform: uppercase;
}

.w-100 {
  width: 100%;
}

.category-text {
  float: right;
  clear: both;
  width: 50%;
  text-align: right;
}

.text-right {
  text-align: right;
}

.pr {
  padding-right: 10px;
}

.pl {
  padding-left: 10px;
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