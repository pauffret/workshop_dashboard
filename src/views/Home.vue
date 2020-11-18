<template>
  <div id="home">
    <div class="title">
      <h1>Bienvenue sur WorkshopDashboard !</h1>
    </div>
      <b-container>

        <b-row class="grid-card justify-content-md-center">
          <b-col cols="4">
            <b-card bg-variant="dark" title="Nombre de personnes dans le batiment :">
              <b-card-text>
                <h1 v-if="compteur.nombrePersonnesActuel != null">{{compteur.nombrePersonnesActuel}}</h1>
                <h1 v-else>0</h1>
              </b-card-text>
              <b-card-text>{{compteur.dateCompteur}}</b-card-text>
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

        <b-row class="grid-card">
          <b-col>
            <b-card bg-variant="dark" title="nb entrée de la semaine">
              <b-card-text>
                Some quick example text to build on the <em>card title</em> and make up the bulk of the card's
                content.
              </b-card-text>
              <b-card-text>A second paragraph of text in the card.</b-card-text>
            </b-card>
          </b-col>
          <b-col>
            <b-card bg-variant="dark" title="nb entrée du mois">
              <b-card-text>
                Some quick example text to build on the <em>card title</em> and make up the bulk of the card's
                content.
              </b-card-text>
              <b-card-text>A second paragraph of text in the card.</b-card-text>
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
      compteur: []
    }
  },
  created() {
    axios.get(`http://192.168.0.2:8080/api/compteur/today`)
        .then(response => {
          this.compteur = response.data
        })
        .catch(e => {
          this.errors.push(e)
        })
  },
}
</script>

<style scoped>

.grid-card {
  padding: 0.5%;
}

</style>