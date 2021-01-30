<template>
  <ion-card v-if="result.length >= 1">
    <ion-card-header>
      <ion-card-subtitle id="subtitle">{{ subtitle }}</ion-card-subtitle>
      <ion-card-title id="result">{{ result }}</ion-card-title>
      <p id="devise">
        <i>{{ devise }}</i>
      </p>
    </ion-card-header>
  </ion-card>
</template>

<script>
import {
  IonCardSubtitle,
  IonCardTitle,
  IonCardHeader,
  IonCard,
} from "@ionic/vue";
export default {
  name: "Result",
  data() {
    return {
      subtitle: "Résultat",
      result: "",
      devise: "",
    };
  },
  components: {
    IonCardSubtitle,
    IonCardTitle,
    IonCardHeader,
    IonCard,
  },
  mounted() {
    this.$bus.on("send-search", ({ result, devise }) => {
      this.result = result;
      this.devise = devise;
    });
  },
};
</script>

<style scoped>
#subtitle {
  margin-bottom: 20px;
}
#title {
  margin-bottom: 0;
}
#devise {
  margin-top: 5px;
}
</style>    