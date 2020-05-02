<template>
  <div>
    <appbar />
    <v-layout column justify-center align-center>
      <div
        v-if="countrySelect"
        class="head display-1 text--secondary text-uppercase"
      >{{selectedCountry}}</div>
      <div v-else class="text-uppercase text--center head">Covid 19 global data</div>
      <div
        class="text-center text--accent-2"
        data-aos="fadeIn"
        data-aos-duration="1500"
        data-aos-delay="300"
      >
        <v-icon color="red" size="15px" class="update">mdi-checkbox-blank-circle</v-icon>
        Updated : {{getDate}}
      </div>
    </v-layout>
    <v-container>
      <v-row v-if="covidData.confirmed">
        <v-col>
          <v-card max-width="300" max-height="400" class="Confirmed iCountUp" outlined shaped>
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
          <v-card max-width="300" max-height="400" class="Active iCountUp" shaped outlined>
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
          <v-card max-width="300" max-height="400" class="Deaths mx-auto"  shaped outlined>
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
          <v-card max-width="300" max-height="400" class="Recovered mx-auto" shaped outlined>
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
        <v-col v-for="n in 4" :key="n" 
      >
          <v-skeleton-loader class="mx-auto" max-width="300" max-height="400" type="image"></v-skeleton-loader>
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
            <h1 class="caption">Scroll to see chart</h1>
            <v-icon class="scrollDown">mdi-chevron-double-down</v-icon>
          </div>
        </v-col>
      </v-row>
    </v-container>
    <section id="Chart">
      <v-container class="chart" >
        <v-row no-gutter >
          <v-col v-if="confirmed">
            <div class="chart">
            <chart
              :confirmed="confirmed"
              :recovered="recovered"
              :deaths="deaths"
              :active="active"
              :country="selectedCountry"
            ></chart>
            </div>
          </v-col>
          <v-col v-else-if="error">
            <v-sheet class="error white--text">{{error}}</v-sheet>
          </v-col>
           <v-col v-else-if="daily" >
             <v-card class="mx-auto" tile >
            <lineChart :dates="this.lastDate" :dailyConfirmed="dailyConfirmed" :dailyDeaths="dailyDeaths" 
            />
             </v-card>
          </v-col>
        </v-row>
      </v-container>
    </section>
  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";
import chart from "../components/chart";
import lineChart from "../components/linechart"
import appbar from "../components/appbar";
import ICountUp from "vue-countup-v2";
import moment from "moment";

// Apis for getting live data
const api = "https://covid19.mathdro.id/api/";
const dailyApi = "https://covid19.mathdro.id/api/daily"

export default {
  components: {
    chart,
    appbar,
    ICountUp,
    lineChart
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
      },
      selectedValues: {},
      selectedCountry: "",
      countrySelect: false,
      countries: [],
      active: 0,
      confirmed: 0,
      recovered: 0,
      deaths: 0,
      covidData: {},
      lastDate:[],
      dailyConfirmed:[],
      dailyDeaths:[],
      dailyRecovered:[],
      daily:false,
      error:'',
    };
  },

  // Getting Updated Dates in a readable format using moment
  computed: {
    getDate() {
     return moment(this.covidData.lastUpdate).format(
        "dddd, MMMM Do YYYY, h:mm a"
      );
    },
  },

  // Calling the methods as soon as compont mounts 
  mounted() {
    this.getGlobalData();
    this.getDailyData()
  },
  //getting name of each countries
  created(){
    axios.get(`${api}countries`).then(res=>{
      const allCountries =res.data.countries;
      allCountries.forEach(country=>this.countries.push(country.name)) 
    })
  },

  // all methods i.e functions 
  methods: {

  // Getting data as soon as user select a country
   checkCountry(country) {
      this.countrySelect = true;
      this.confirmed = 0;
       axios
        .get(`${api}countries/${country}`)
        .then(res => {
          this.confirmed = res.data.confirmed.value;
          this.recovered = res.data.recovered.value;
          this.deaths = res.data.deaths.value;
          // calculating the active cases values
          this.active =
            res.data.confirmed.value -
            res.data.recovered.value -
            res.data.deaths.value;
        })
        .catch(err => this.error=`Data not found ${err}`);
    },

  // calling reset for showing global data
    reset() {
      this.confirmed = 0;
      this.countrySelect = false;
      this.selectedCountry = "";
      this.error=''
    },

  // Getting global data
   getGlobalData() {
      axios.get(`${api}`).then(res => {
        this.covidData = res.data;
      });
    },

    //Getting daily updates for line chart
     getDailyData() {
      axios.get(`${dailyApi}`).then(res => {
        const data=res.data;
        this.daily=true
        data.forEach(data=>{
          this.lastDate.push(data.reportDate)
          this.dailyConfirmed.push(data.confirmed.total)
          this.dailyDeaths.push(data.deaths.total)
        })
      });
    },

    onReady: function(instance, CountUp) {
      const that = this;
      instance.update(that.endVal + 100);
    }
  }
};
</script>


<style scope>
.Active {
  border-bottom: 3px solid rgb(247, 156, 171) !important;
}
.Confirmed {
  border-bottom: 3px solid rgb(51, 104, 216) !important;
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

.update {
  animation: blink 1s infinite alternate;
}

.scrollDown {
  transition: all ease-in-out;
  animation: move 0.6s alternate infinite;
}

@keyframes move {
  to {
    transform: translateY(20px);
  }
}

@keyframes blink {
  to {
    transform: scale(1.4);
  }
}


</style>