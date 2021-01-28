<template>
  <ion-page>
    <ion-header translucent>
      <ion-toolbar>
        <ion-title>Convertisseur </ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <!--
      <div id="container">
        <strong>Ready to create an app?</strong>
        <p>Start with Ionic <a target="_blank" rel="noopener noreferrer" href="https://ionicframework.com/docs/components">UI Components</a></p>
      </div>-->
      <div id="container">
        <form @submit.prevent="">
          <!--
          <ion-grid>
            <ion-row class="ion-justify-content-start">
              <ion-col size="3">
                <div>1 of 2</div>
              </ion-col>
              <ion-col size="3">
                <div>2 of 2</div>
              </ion-col>
            </ion-row>
          </ion-grid>-->

          <ion-item class="ion-margin-end">
            <ion-input
              v-model="dataInputAmout"
              type="number"
              placeholder="Montant"
            ></ion-input>
            <ion-label>€</ion-label>
          </ion-item>
          <img id="convert-icon" src="../asset/caret-down-outline.svg" alt="" />
          <ion-item class="ion-margin-end">
            <ion-select v-model="dataSelectEntry" class="margin-auto">
              <ion-select-option
                v-for="symbol in symbolsResult"
                :key="symbol"
                value="1"
                >{{ symbol }}</ion-select-option
              >
              <ion-select-option value="1.210265">USD</ion-select-option>
            </ion-select>
          </ion-item>
          <ion-item class="ion-margin-end">
            <ion-select v-model="dataSelectExit" class="margin-auto">
              <ion-select-option value="1">EURO</ion-select-option>
              <ion-select-option value="1.210265">USD</ion-select-option>
            </ion-select>
          </ion-item>
          <h1 @click="valueSelect">Submit</h1>
          <p>
            {{ dataInputAmout }}, {{ dataSelectEntry }}, {{ dataSelectExit }}
          </p>
        </form>
      </div>
    </ion-content>
  </ion-page>
</template>

<script>
import {
  IonTitle,
  IonContent,
  IonHeader,
  IonPage,
  IonToolbar,
  IonInput,
  IonItem,
  IonLabel,
  IonSelect,
  IonSelectOption,
} from "@ionic/vue";
import axios from "axios";

export default {
  name: "Home",
  components: {
    IonTitle,
    IonContent,
    IonHeader,
    IonPage,
    IonToolbar,
    IonInput,
    IonItem,
    IonLabel,
    IonSelect,
    IonSelectOption,
  },
  data() {
    return {
      dataInputAmout: null,
      dataSelectEntry: null,
      dataSelectExit: null,
      symbolsResult: [],
    };
  },
  mounted() {
    axios
      .get(
        `http://data.fixer.io/api/symbols?access_key=3d48e8e331db43fe1490ceef3feb882f`
      )
      .then((response) => {
        const symbolsResult = response.data.symbols;
        this.symbolsResult = symbolsResult;
      })
      .catch((error) => {
        console.log(error);
      });
  },
  methods: {
    valueSelect() {
      console.log(
        this.getValueFromEntryCurrencyToExitCurrency(
          this.dataSelectEntry,
          this.dataInputAmout,
          this.dataSelectExit
        )
      );
    },
    getValueFromEntryCurrencyToExitCurrency(
      entryCurrency,
      entryCurrencyAmount,
      exitCurrency
    ) {
      const result =
        Math.round(
          ((exitCurrency * entryCurrencyAmount) / entryCurrency) * 100
        ) / 100;

      return result;
    },
  },
};
</script>

<style scoped>
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

ion-label {
  margin: 0;
}

.margin-auto {
  margin: auto;
}

#convert-icon {
  width: 30px;
  margin: 20px;
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  color: #8c8c8c;
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>