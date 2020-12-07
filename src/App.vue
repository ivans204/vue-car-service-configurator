<template>
  <div id="app" class="main-container">
    <Navbar></Navbar>
    <hr>
    <div class="content">
      <p class="align-center">Pritisnite gumb za pokretanje konfiguratora</p>
      <button @click="modalActive = true">Pokreni konfigurator</button>

      {{ configuratorData }}

      <Modal v-if="modalActive" @closeModal="resetModal()">
        <component :is="componentType"
                   @changeComponent="changeComp($event)"
                   @saveCarModel="saveData('carModel', $event)"
                   @saveCarServiceData="saveData('selectedServices', $event)"
                   @saveServicePrice="saveData('priceData', $event)"
                   @saveContactDetails="saveData('contactDetails', $event)"
                   @closeModal="resetModal()"
                   :configurator-data="configuratorData"
        ></component>
      </Modal>

    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar";
import Modal from "@/components/Modal";
import CarModel from "@/components/CarModel";
import CarService from "@/components/CarService";
import ContactForm from "@/components/ContactForm";
import DetailsReview from "@/components/DetailsReview";
import ConfigSent from "@/components/ConfigSent";

export default {
  components: {
    Navbar,
    Modal,
    CarModel,
    CarService,
    ContactForm,
    DetailsReview,
    ConfigSent
  },
  name: 'App',
  data: () => ({
    modalActive: false,
    componentType: 'CarModel',
    configuratorData: {}
  }),
  methods: {
    resetModal() {
      this.modalActive = false;
      this.configuratorData = {}
      this.componentType = 'CarModel'
    },
    changeComp(newComponent) {
      this.componentType = newComponent;
    },
    saveData(propName, emittedData) {
      this.configuratorData[propName] = emittedData
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}
</style>

<style lang="scss" scoped>
.main-container {
  width: 100%;
  position: relative;
}

.content {
  display: flex;
  flex-direction: column;

  > button {
    width: 150px;
    display: block;
    margin: 10px auto;
  }
}

.align-center {
  text-align: center;
}
</style>
