<template>
  <div>
    <appbar />
    <v-layout column justify-center align-center>
      <div v-if="countrySelect" class=" head display-1 text--secondary text-uppercase">{{selectedCountry}}</div>
      <div v-else class="text-uppercase text--center head">Covid 19 global data</div>
      <div class="text-center text--accent-2" >{{covidData.lastUpdate}}</div>
    </v-layout>
    <v-container >
      <v-row class="mx-auto">
        <v-col cols="12" md="4" lg="4">
          <v-card max-width="300" max-height="400" class="Confirmed mx-1">
            <v-card-text>
              <div class="text-center text-capitalize display-2">Active</div>
              <div class="text-center text--accent-2" v-if="countrySelect">{{confirmed}}</div>
              <div class="text-center text--accent-2" v-else>{{covidData.confirmed.value}}</div>
            </v-card-text>
          </v-card>
        </v-col>
        <v-col cols="12" md="4" lg="4">
          <v-card max-width="300" max-height="400" class="Deaths">
            <v-card-text>
              <div class="text-center text-capitalize display-2">Deaths</div>
              <div class="text-center text--accent-2" v-if="countrySelect">{{deaths}}</div>
              <div class="text-center text--accent-2" v-else>{{covidData.deaths.value}}</div>
            </v-card-text>
          </v-card>
        </v-col>
        <v-col cols="12" md="4" lg="4">
          <v-card max-width="300" max-height="400" class="Recovered">
            <v-card-text>
              <div class="text-center text-capitalize display-2">Recovered</div>
              <div class="text-center text--accent-2" v-if="countrySelect">{{recovered}}</div>
              <div class="text-center text--accent-2" v-else>{{covidData.recovered.value}}</div>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
      <div>Select a country to display</div>
      <v-row align="center">
        <v-col>
          <v-select
            :items="country"
            v-model="selectedCountry"
            label="Country"
            @change="checkCountry(selectedCountry)"
          ></v-select>
          <div>
            <v-btn @click="reset">reset</v-btn>
          </div>
        </v-col>
      </v-row>
      <v-row no-gutter>
        <v-col v-if="confirmed">
          <chart :confirmed="confirmed" :recovered="recovered" :deaths="deaths"></chart>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import axios from "axios";
import chart from "../components/chart";
import appbar from "../components/appbar";
export default {
  components: {
    chart,
    appbar
  },
  data() {
    return {
      selectedValues: {},
      selectedCountry: "",
      countrySelect: false,
      country: [
        "Afghanistan",
        "Albania",
        "Algeria",
        "Andorra",
        "Angola",
        "Anguilla",
        "Antigua &amp; Barbuda",
        "Argentina",
        "Armenia",
        "Aruba",
        "Australia",
        "Austria",
        "Azerbaijan",
        "Bahamas",
        "Bahrain",
        "Bangladesh",
        "Barbados",
        "Belarus",
        "Belgium",
        "Belize",
        "Benin",
        "Bermuda",
        "Bhutan",
        "Bolivia",
        "Bosnia &amp; Herzegovina",
        "Botswana",
        "Brazil",
        "British Virgin Islands",
        "Brunei",
        "Bulgaria",
        "Burkina Faso",
        "Burundi",
        "Cambodia",
        "Cameroon",
        "Cape Verde",
        "Cayman Islands",
        "Chad",
        "Chile",
        "China",
        "Colombia",
        "Congo",
        "Cook Islands",
        "Costa Rica",
        "Cote D Ivoire",
        "Croatia",
        "Cruise Ship",
        "Cuba",
        "Cyprus",
        "Czech Republic",
        "Denmark",
        "Djibouti",
        "Dominica",
        "Dominican Republic",
        "Ecuador",
        "Egypt",
        "El Salvador",
        "Equatorial Guinea",
        "Estonia",
        "Ethiopia",
        "Falkland Islands",
        "Faroe Islands",
        "Fiji",
        "Finland",
        "France",
        "French Polynesia",
        "French West Indies",
        "Gabon",
        "Gambia",
        "Georgia",
        "Germany",
        "Ghana",
        "Gibraltar",
        "Greece",
        "Greenland",
        "Grenada",
        "Guam",
        "Guatemala",
        "Guernsey",
        "Guinea",
        "Guinea Bissau",
        "Guyana",
        "Haiti",
        "Honduras",
        "Hong Kong",
        "Hungary",
        "Iceland",
        "India",
        "Indonesia",
        "Iran",
        "Iraq",
        "Ireland",
        "Isle of Man",
        "Israel",
        "Italy",
        "Jamaica",
        "Japan",
        "Jersey",
        "Jordan",
        "Kazakhstan",
        "Kenya",
        "Kuwait",
        "Kyrgyz Republic",
        "Laos",
        "Latvia",
        "Lebanon",
        "Lesotho",
        "Liberia",
        "Libya",
        "Liechtenstein",
        "Lithuania",
        "Luxembourg",
        "Macau",
        "Macedonia",
        "Madagascar",
        "Malawi",
        "Malaysia",
        "Maldives",
        "Mali",
        "Malta",
        "Mauritania",
        "Mauritius",
        "Mexico",
        "Moldova",
        "Monaco",
        "Mongolia",
        "Montenegro",
        "Montserrat",
        "Morocco",
        "Mozambique",
        "Namibia",
        "Nepal",
        "Netherlands",
        "Netherlands Antilles",
        "New Caledonia",
        "New Zealand",
        "Nicaragua",
        "Niger",
        "Nigeria",
        "Norway",
        "Oman",
        "Pakistan",
        "Palestine",
        "Panama",
        "Papua New Guinea",
        "Paraguay",
        "Peru",
        "Philippines",
        "Poland",
        "Portugal",
        "Puerto Rico",
        "Qatar",
        "Reunion",
        "Romania",
        "Russia",
        "Rwanda",
        "Saint Pierre &amp; Miquelon",
        "Samoa",
        "San Marino",
        "Satellite",
        "Saudi Arabia",
        "Senegal",
        "Serbia",
        "Seychelles",
        "Sierra Leone",
        "Singapore",
        "Slovakia",
        "Slovenia",
        "South Africa",
        "South Korea",
        "Spain",
        "Sri Lanka",
        "St Kitts &amp; Nevis",
        "St Lucia",
        "St Vincent",
        "St. Lucia",
        "Sudan",
        "Suriname",
        "Swaziland",
        "Sweden",
        "Switzerland",
        "Syria",
        "Taiwan",
        "Tajikistan",
        "Tanzania",
        "Thailand",
        "Timor L'Este",
        "Togo",
        "Tonga",
        "Trinidad &amp; Tobago",
        "Tunisia",
        "Turkey",
        "Turkmenistan",
        "Turks &amp; Caicos",
        "Uganda",
        "Ukraine",
        "United Arab Emirates",
        "United Kingdom",
        "Uruguay",
        "Uzbekistan",
        "Venezuela",
        "Vietnam",
        "Virgin Islands (US)",
        "Yemen",
        "Zambia",
        "Zimbabwe"
      ],
      confirmed: 0,
      recovered: 0,
      deaths: 0,
      covidData: {}
    };
  },
  mounted() {
    this.getGlobalData();
  },
  methods: {
    checkCountry(country) {
      this.countrySelect = true;
      this.confirmed = 0;
      axios
        .get(`https://covid19.mathdro.id/api/countries/${country}`)
        .then(res => {
          this.confirmed = res.data.confirmed.value;
          this.recovered = res.data.recovered.value;
          this.deaths = res.data.deaths.value;
        });
    },

    reset() {
      this.confirmed = 0;
      this.countrySelect = false;
    },

    getGlobalData() {
      axios.get(`https://covid19.mathdro.id/api`).then(data => {
        this.covidData = data.data;
      });
    }
  }
};
</script>


<style scoped>
.Confirmed {
  border-bottom: 3px solid violet !important;
}
.Deaths {
  border-bottom: 3px solid red !important;
}
.Recovered {
  border-bottom: 3px solid green !important;
}

.head{
  font-size: 30px;
}
</style>