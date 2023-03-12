<template>
  <div class="box">
    <div>
      <span>Country:</span>
      <select v-model="selected" style="width: 250px">
        <option disabled value="">Select a country</option>
        <option v-for="country in allData" :key="country.name">
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
    
      // getting data via API
      await axios
        .get("https://countriesnow.space/api/v0.1/countries/capital")
        
        // if API sends back a successful response...
        .then((response) => {
          if (
            response.status == 200 &&
            response.data.error == false &&
            response.data.msg == "countries and capitals retrieved"
          ) {
 
            // store the response data in an empty array
            this.allData = response.data.data;

            // clean the incoming data
            // discard those objects which don't have capitals
            this.allData = this.allData.filter((c) => c.capital != "");

          }
        })
        
        // handles any error & displays the appropriate message on browser console
        .catch((error) =>
          console.log(error.response.data.msg, "\n", error.message)
        );
    },
    onClick() {
    
      // if the user doesn't select a country and directly clicks the button, alert is shown
      if (this.selected == "" || this.selected == null) {
        alert("Please select a country");
      } 
      
      // once the user selects a country, displaying the corresponding capital
      else {
        const result = this.allData.filter((c) =>
          c.name.includes(this.selected)
        );
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
