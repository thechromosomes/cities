<template>
  <!-- app -->
  <div class="modal-vue">
    <!-- overlay -->
    <div class="overlay"></div>

    <!-- modal -->
    <div class="modal">
      <button class="close" @click="$emit('close-popup')">x</button>
      <h3>{{ country.name.common }} {{ country.flag }}</h3>
      <div>Official: {{ country.name.official }}</div>
      <div>currencies: {{ country.name.official }}</div>
      <div>capital: {{ country.capital[0] }}</div>
      <div>region: {{ country.region }}</div>
      <div>timezones: {{ country.timezones[0] }}</div>
    </div>
  </div>
</template>
<script>
export default {
  props: ["country"],
  emits: ["close-popup"],
};
</script>

<style scoped>
.modal-vue .overlay {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal-vue .modal {
  position: relative;
  width: 300px;
  z-index: 9999;
  margin: 0 auto;
  padding: 20px 30px;
  background-color: #fff;
}

.modal-vue .close {
  position: absolute;
  top: 10px;
  right: 10px;
}
</style>