<template>
  <div>
    <appbar />
    <v-layout column justify-center align-center>
      <div
        v-if="countrySelect"
        class="head display-1 text--secondary text-uppercase"
      >{{selectedCountry}}</div>
      <div
        v-else
        class="text-uppercase text--center head"
      >Covid 19 global data</div>
      <div
        class="text-center text--accent-2"
        data-aos="fade-up"
        data-aos-duration="1500"
        data-aos-delay="300"
      >Updated : {{getDate}}</div>
    </v-layout>
    <v-container >
      <v-row v-if="covidData.confirmed">
      <v-col > 
          <v-card
            max-width="300"
            max-height="400"
            class="Confirmed iCountUp"
            data-aos="fade-up"
            data-aos-duration="1000"
            data-aos-delay="50"
          >
            <v-card-text>
              <div class="text-center display-2">
                <v-chip color="indigo" text-color="white">
                  <v-icon>mdi-account-check</v-icon>Confirmed
                </v-chip>
              </div>
              <div class="text-center text--accent-2" v-if="countrySelect">
                <ICountUp
                  :delay="delay"
                  :endVal="confirmed"
                  :options="options"
                  class="text-center text--accent-2"
                />
              </div>
              <div class="text-center text--accent-2" v-else>
                <ICountUp
                  :delay="delay"
                  :endVal="covidData.confirmed.value"
                  :options="options"
                  class="text-center text--accent-2"
                />
              </div>
            </v-card-text>
          </v-card>
        </v-col>
        <v-col>
          <v-card
            max-width="300"
            max-height="400"
            class="Active iCountUp"
            data-aos="fade-up"
            data-aos-duration="1000"
            data-aos-delay="50"
          >
            <v-card-text>
              <div class="text-center display-2">
                <v-chip color="pink" text-color="white">
                  <v-icon>mdi-account-convert</v-icon>Active
                </v-chip>
              </div>
              <div class="text-center" v-if="countrySelect">
                <ICountUp :delay="delay" :endVal="active" :options="options" class="text-center" />
              </div>
              <div class="text-center" v-else>
                <ICountUp
                  :delay="delay"
                  :endVal="covidData.confirmed.value-covidData.recovered.value-covidData.deaths.value"
                  :options="options"
                  class="text-center"
                />
              </div>
            </v-card-text>
          </v-card>
        </v-col>
        <v-col>
          <v-card
            max-width="300"
            max-height="400"
            class="Deaths mx-auto"
            data-aos="fade-up"
            data-aos-duration="1000"
            data-aos-delay="300"
          >
            <v-card-text>
              <div class="text-center display-2">
                <v-chip color="red" text-color="white">
                  <v-icon>mdi-account-off</v-icon>Deaths
                </v-chip>
              </div>
              <div class="text-center" v-if="countrySelect">
                <ICountUp :delay="delay" :endVal="deaths" :options="options" />
              </div>
              <div class="text-center" v-else>
                <ICountUp :delay="delay" :endVal="covidData.deaths.value" :options="options" />
              </div>
            </v-card-text>
          </v-card>
        </v-col>
        <v-col>
          <v-card
            max-width="300"
            max-height="400"
            class="Recovered mx-auto"
            data-aos="fade-up"
            data-aos-duration="1000"
            data-aos-delay="500"
          >
            <v-card-text>
              <div class="text-center display-2">
                <v-chip color="green" text-color="white">
                  <v-icon>mdi-account-multiple-plus</v-icon>Recovered
                </v-chip>
              </div>
              <div class="text-center" v-if="countrySelect">
                <ICountUp :delay="delay" :endVal="recovered" :options="options" />
              </div>
              <div class="text-center" v-else>
                <ICountUp :delay="delay" :endVal="covidData.recovered.value" :options="options" />
              </div>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
     <v-row v-else>
       <v-col>
      <h1 class="text-center">loading</h1>
       </v-col>
      </v-row>
      <div>Select a country to display</div>
      <v-row align="center">
        <v-col>
          <v-select
            :items="countries"
            v-model="selectedCountry"
            label="Country"
            @change="checkCountry(selectedCountry)"
          ></v-select>
          <div>
            <v-btn @click="reset" color="primary">reset</v-btn>
          </div>
          <div v-if="countrySelect" class="text-center">
              <h1 class="caption ">Scroll to see chart</h1>
              <v-icon class="scrollDown">mdi-chevron-double-down</v-icon>
          </div>
        </v-col>
      </v-row>
    </v-container>
    <section id="Chart">
      <v-container>
       <v-row no-gutter>
        <v-col v-if="confirmed || active">
          <chart
            :confirmed="confirmed"
            :recovered="recovered"
            :deaths="deaths"
            :active="active"
            :country="selectedCountry"
          ></chart>
        </v-col>
      </v-row>
      </v-container>
    </section>
  </div>
</template>

<script>
import Vue from 'vue';
import axios from "axios";
import chart from "../components/chart";
import appbar from "../components/appbar";
import ICountUp from "vue-countup-v2";
import moment from "moment";
const api="https://covid19.mathdro.id/api/";
export default {
  components: {
    chart,
    appbar,
    ICountUp
  },
  data() {
    return {
      delay: 1000,
      height: 300,
      options: {
        useEasing: true,
        useGrouping: true,
        separator: ",",
        decimal: ".",
        prefix: "",
        suffix: ""
      },
      selectedValues: {},
      selectedCountry: "",
      countrySelect: false,
      countries: [
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
        "USA",
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
      active: 0,
      confirmed: 0,
      recovered: 0,
      deaths: 0,
      covidData: {},
    };
  },
  computed: {
    getDate() {
      const date = moment(this.covidData.lastUpdate).format(
        "dddd, MMMM Do YYYY, h:mm a"
      );
      return date;
    },
  },
  mounted() {
     this.getGlobalData();
  },
  methods: {
    async checkCountry(country) {
      this.countrySelect = true;
      this.confirmed = 0;
      await axios
        .get(`${api}countries/${country}`)
        .then(res => {
          this.confirmed = res.data.confirmed.value;
          this.recovered = res.data.recovered.value;
          this.deaths = res.data.deaths.value;
          this.active =
            res.data.confirmed.value -
            res.data.recovered.value -
            res.data.deaths.value;
        }
      ).catch(err=>console.log(err));
    },

    reset() {
      this.confirmed = 0
      this.countrySelect = false
      this.selectedCountry=""
    },

    async getGlobalData() {
     const data= await axios.get(`${api}`).then(res => {
        this.covidData = res.data
      });
    },

    onReady: function(instance, CountUp) {
      const that = this;
      instance.update(that.endVal + 100)
    }
  }
};
</script>


<style scoped>
.Active {
  border-bottom: 3px solid pink !important;
}
.Confirmed {
  border-bottom: 3px solid violet !important;
}
.Deaths {
  border-bottom: 3px solid red !important;
}
.Recovered {
  border-bottom: 3px solid green !important;
}

.head {
  font-size: 30px;
}
.scrollDown{
  animation: move 0.6s ease-in-out alternate infinite;
}

@keyframes move {
  to{
    transform: translateY(20px);
  }
}

</style>