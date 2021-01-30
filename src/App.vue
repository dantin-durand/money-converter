<template>
  <ion-app>
    <ion-page>
      <ion-content :fullscreen="true">
        <div id="container">
          <img id="logo" src="./asset/logo.svg" alt="logo vue stonks" />
          <Search />
          <Result />
        </div>
      </ion-content>
    </ion-page>
  </ion-app>
</template>

<script>
import { IonApp, IonPage, IonContent } from "@ionic/vue";
import Result from "./views/Result.vue";
import Search from "./views/Search.vue";

export default {
  name: "App",
  components: {
    IonApp,
    IonPage,
    IonContent,
    Search,
    Result,
  },
};
</script>

<style scoped>
ion-content {
  --background: linear-gradient(to top, #0ba360 0%, #5ae0b6 100%);
}
#container {
  text-align: center;
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
  width: 80%;
  margin: auto;
}
#logo {
  width: 200px;
  margin-bottom: 50px;
}
</style>