<template>
  <div>
    <p class="configurator-step">Korak 1: Odaberite poizvođača vašeg vozila</p>

    <div class="car-model-wrap">
      <span class="car-model-input" v-for="car in carModels" :key="car">
      <input type="radio"
             :id="car"
             :value="car"
             v-model="pickedModel"
             :checked="pickedModel"
      >
      <label :for="car">{{ car }}</label>
      </span>
    </div>

    <div class="controlButtons">
      <button @click="nextStep" :disabled="!pickedModel">Dalje</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "CarModel",
  props: {
    configuratorData: Object
  },
  data: () => ({
    carModels: ['Peugeot', 'Volkswagen', 'Citroen', 'Audi', 'Bmw', 'Seat', 'Alfa Romeo', 'Kia', 'Hyundai', 'Honda', 'Toyota'],
    pickedModel: '',
  }),
  methods: {
    nextStep() {
      this.$emit('saveCarModel', this.pickedModel)
      this.$emit('changeComponent', 'CarService')
    }
  },
  mounted() {
    this.configuratorData ? this.pickedModel = this.configuratorData.carModel : this.pickedModel = ''
  }
}
</script>

<style scoped lang="scss">

.configurator-step {
  margin: 30px 0;
  font-size: 1.2rem;
}

.car-model-wrap {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  padding-bottom: 150px;
}

.car-model-input {
  flex: 0 1 15%;
  margin-right: 10px;
  margin-bottom: 15px;
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