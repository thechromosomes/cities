<template>
  <div class="hello">
    <h1>HelloWorld</h1>
    <input
      v-model="inpuText"
      placeholder="Type City Name"
      id="inputPlaceHolder"
    />
    <ul>
      <template v-if="filterdArray().length">
        <template v-if="modal">
          <CountryModal @close-popup="toggleModal" :country="selectedCountry">
          </CountryModal>
        </template>
        <li v-for="(item, index) in filterdArray().splice(0, 10)" :key="index">
          <div
            class="card"
            v-html="item"
            @click="getCountryDetail(item, index)"
          ></div>
        </li>
      </template>
      <template v-else-if="!countryList.length">
        <li>
          <div class="loader"></div>
        </li>
      </template>
    </ul>
  </div>
</template>

<script>
import CountryModal from "./CountryModal.vue";
export default {
  components: { CountryModal },
  data() {
    return {
      countryList: [],
      inpuText: "",
      countryDetail: [],
      selectedCountry: [],
      modal: false,
    };
  },
  async created() {
    let url = "https://restcountries.com/v3.1/all";
    let temp = await fetch(url);
    let tempParse = await temp.json();
    let cities = tempParse.map((elem) => elem.name.common);
    this.countryDetail = tempParse;
    this.countryList = cities;
    // document.getElementById("inputPlaceHolder").focus();
  },

  methods: {
    toggleModal() {
      this.modal = !this.modal;
    },
    getCountryDetail(payload, index) {
      this.countryDetail.filter((elem) => {
        if (
          elem.name.common.toLowerCase() ===
          this.filterdArray("nameOnly")[index].toLowerCase()
        ) {
          this.selectedCountry = elem;
          this.modal = true;
        }
      });
    },
    filterdArray(behave) {
      let temp =
        this.countryList.filter((elem) =>
          elem.toLowerCase().includes(this.inpuText.toLowerCase())
        ) || this.countryList;
      let temp2 = temp.map((temp) => {
        let elem = temp.toLowerCase();
        if (behave === "nameOnly") {
          return elem;
        }
        return elem.replaceAll(
          elem.match(this.inpuText.toLowerCase()),
          `<b class="red">${elem.match(this.inpuText)}</b>`
        );
      });
      return temp2;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
input {
  padding: 10px;
}
.loader {
  border: 10px solid #f3f3f3; /* Light grey */
  border-top: 10px solid #3498db; /* Blue */
  border-radius: 50%;
  width: 50px;
  height: 50px;
  animation: spin 2s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
.card {
  cursor: pointer;
  border: solid;
  padding: 10px;
  margin: 10px;
  text-transform: uppercase;
}
.red {
  color: red;
}
b {
  color: red;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
