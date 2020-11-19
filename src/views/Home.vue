<template>
  <div id="home">
    <div class="title">
      <h1>Bienvenue sur WorkshopDashboard !</h1>
    </div>
      <b-container class="first-container">

        <b-row class="grid-card justify-content-md-center">
          <b-col cols="4">
            <b-card bg-variant="dark" title="Nombre actuel de personnes dans le batiment :">
              <b-card-text>
                <h1 v-if="compteur.nombrePersonnesActuel != null">{{compteur.nombrePersonnesActuel}}</h1>
                <h1 v-else>0</h1>
              </b-card-text>
            </b-card>
          </b-col>
        </b-row>

        <b-row class="grid-card">
          <b-col>
            <b-card bg-variant="dark" title="Nombre d'entrées aujourd'hui :">
              <b-card-text>
                <h1 v-if="compteur.nombreEntrees != null">{{compteur.nombreEntrees}}</h1>
                <h1 v-else>0</h1>
              </b-card-text>
            </b-card>
          </b-col>
          <b-col>
            <b-card bg-variant="dark" title="Nombre de sorties aujourd'hui :">
              <b-card-text>
                <h1 v-if="compteur.nombreSorties != null">{{compteur.nombreSorties}}</h1>
                <h1 v-else>0</h1>
              </b-card-text>
            </b-card>
          </b-col>
        </b-row>

        <!------------------------------------------------------------------------->

        <div class="separate-grid">
          <h3>Sélectionner une date pour accéder à ses informations</h3>
        </div>

        <b-row class="grid-card">
          <b-col md="auto">
            <b-calendar v-model="value" @context="onContext" locale="en-US"></b-calendar>
          </b-col>
          <b-col class="select-date">
            <b-card bg-variant="dark" title="Nombre d'entrées du jour sélectionné :">
              <b-card-text>
                <h1 v-if="compteurDate.nombreEntrees != null">{{compteurDate.nombreEntrees}}</h1>
                <h1 v-else>0</h1>
              </b-card-text>
            </b-card>
          </b-col>
          <b-col class="select-date">
            <b-card bg-variant="dark" title="Nombre de sorties du jour sélectionné :">
              <b-card-text>
                <h1 v-if="compteurDate.nombreSorties != null">{{compteurDate.nombreSorties}}</h1>
                <h1 v-else>0</h1>
              </b-card-text>
            </b-card>
          </b-col>
          <b-col class="select-date">
            <b-card bg-variant="dark" title="Nombre d'entrées du mois :">
              <b-card-text>
                <h1 v-if="compteurMoisEntrees != null">{{compteurMoisEntrees}}</h1>
                <h1 v-else>0</h1>
              </b-card-text>
            </b-card>
          </b-col>
        </b-row>

      </b-container>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Home",
  data() {
    return {
      compteur: [],
      compteurDate: [],
      compteurMois: [],
      compteurMoisEntrees: 0,
      value: '',
      context: null
    }
  },
  created() {
    this.apiCallCompteurs()
    setInterval(() => {
      this.apiCallCompteurs()
    }, 3000)
  },
  watch: {
    value: function (val){
      this.compteurMoisEntrees = 0
      axios.get(`http://192.168.0.2:8080/api/compteur/date/`+val)
          .then(response => (
              this.compteurDate = response.data
          ))
          .catch(e => {
            this.errors.push(e)
          })
      axios.get(`http://192.168.0.2:8080/api/compteurs/mois/`+val.split("-")[0]+`/`+val.split("-")[1])
          .then(response => (
              this.compteurMois = response.data,
              this.getEntreesMois()
          ))
          .catch(e => {
            this.errors.push(e)
          })
    }
  },
  methods: {
    onContext(ctx) {
      this.context = ctx
    },
    apiCallCompteurs(){
      axios.get(`http://192.168.0.2:8080/api/compteur/today`)
          .then(response => (
              this.compteur = response.data
          ))
          .catch(e => {
            this.errors.push(e)
          })
    },
    getEntreesMois() {
      for (let i=0; i < this.compteurMois.length; i++) {
        if (this.compteurMois[i].nombreEntrees) {
          this.compteurMoisEntrees += this.compteurMois[i].nombreEntrees
        }
      }
    }
  }
}
</script>

<style scoped>

.first-container {
  padding-top: 1%;
}

.grid-card {
  padding: 0.5%;
}

.separate-grid {
  padding-top: 5%;
}

.select-date {
  min-height: 200%;
}

</style>