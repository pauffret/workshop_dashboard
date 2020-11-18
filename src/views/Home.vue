<template>
  <div id="home">
    <div class="title">
      <h1>Bienvenue sur WorkshopDashboard !</h1>
    </div>
      <b-container class="bv-example-row">
        <b-row>

          <b-col>
            <b-card title="nb personnes dans le batiment" sub-title="Card subtitle">
              <b-card-text>
                {{compteur.nombrePersonnesActuel}}
              </b-card-text>
              <b-card-text>{{compteur.dateCompteur}}</b-card-text>
            </b-card>
          </b-col>

          <b-col>
            <b-card title="nb d'entrée" sub-title="Card subtitle">
              <b-card-text>
                {{compteur.nombreEntrees}}
              </b-card-text>
            </b-card>
          </b-col>

          <b-col>
            <b-card title="nb sortie" sub-title="Card subtitle">
              <b-card-text>
                {{compteur.nombreSorties}}
              </b-card-text>
            </b-card>
          </b-col>

        </b-row>

        <b-row>

          <b-col>
            <b-card title="nb entrée de la semaine" sub-title="Card subtitle">
              <b-card-text>
                Some quick example text to build on the <em>card title</em> and make up the bulk of the card's
                content.
              </b-card-text>
              <b-card-text>A second paragraph of text in the card.</b-card-text>
            </b-card>
          </b-col>

          <b-col>
            <b-card title="nb entrée du mois" sub-title="Card subtitle">
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

</style>