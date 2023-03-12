<template>
  <div class="box">
    <div>
      <span>Country:</span>
      <!-- <input type="text" v-model="country" /><br /><br /> -->
      <select v-model="selected" style="width: 250px">
        <option disabled value="">Select a country</option>
        <option v-for="country in allData" :key="country.index">
          {{ country.name }}
        </option></select
      ><br /><br />
    </div>
    <div>
      <span>Capital:</span>
      <input
        type="text"
        v-model="capital"
        disabled
        style="width: 250px"
      /><br /><br />
    </div>
    <div><button @click="onClick()">Click</button><br /><br /></div>
    <!-- {{ selected }} -->
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      selected: "",
      capital: "",
      allData: [],
    };
  },
  created() {
    this.getData();
  },
  methods: {
    async getData() {
      await axios
        .get("https://countriesnow.space/api/v0.1/countries/capital")
        .then((response) => {
          if (
            response.status == 200 &&
            response.data.error == false &&
            response.data.msg == "countries and capitals retrieved"
          ) {
            //console.log(response.data.data);
            // const result = response.data.data;
            // this.allData = result;

            // console.log("Show all data inside array",this.allData);
            // for(let i in this.allData){
            //   console.log("Country-",this.allData[i].name);
            // }
            this.allData = response.data.data;
            //console.log(this.allData.length);
            //console.log(this.allData[0]);
            // console.log(this.allData.filter((c) => c.capital == ""));
            // console.log(this.allData.filter((c) => c.capital != ""));
            this.allData = this.allData.filter((c) => c.capital != "");
            // console.log(typeof response.data.error);
            // console.log(typeof response.status);
          }
        })
        .catch((error) =>
          console.log(error.response.data.msg, "\n", error.message)
        );
    },
    onClick() {
      if (this.selected == "" || this.selected == null) {
        alert("Please select a country");
      } else {
        const result = this.allData.filter((c) =>
          c.name.includes(this.selected)
        );
        // console.log("Capital:",result[0].capital);
        this.capital = result[0].capital;
      }
    },
  },
};
</script>

<style scoped>
.box {
  margin: auto;
  background-color: lightgrey;
  width: 400px;
  border: 10px solid green;
  padding: 50px;
}
</style>