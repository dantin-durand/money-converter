<template>
  <ion-app>
    <home></home>
  </ion-app>
</template>

<script>
import { IonApp } from "@ionic/vue";
import Home from "./views/Home.vue";

export default {
  name: "App",
  components: {
    IonApp,
    Home,
  },
};
</script>