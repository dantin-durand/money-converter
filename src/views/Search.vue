<template>
  <form @submit.prevent="">
    <ion-item class="input-amout">
      <ion-input
        class="ion-text-center"
        v-model="dataInputAmout"
        type="number"
        placeholder="Montant"
      ></ion-input>
      <ion-label class="label-amout">{{ dataSelectEntry }}</ion-label>
    </ion-item>
    <ion-grid>
      <ion-row class="row-selector">
        <ion-col class="col-selector">
          <p class="title-selector">Devise d'entrée</p>
          <ion-item class="item-select-entry">
            <ion-select
              interface="action-sheet"
              cancel-text="Retour"
              v-model="dataSelectEntry"
              class="margin-auto"
            >
              <ion-select-option
                v-for="(devise, index) in devises"
                :key="devise"
                :value="index"
                >{{ index }} - {{ devise }}</ion-select-option
              >
            </ion-select>
          </ion-item></ion-col
        >
        <ion-col class="col-selector">
          <p class="title-selector">Devise de sortie</p>
          <ion-item class="item-select-exit">
            <ion-select
              interface="action-sheet"
              v-model="dataSelectExit"
              class="margin-auto"
            >
              <ion-select-option
                v-for="(devise, index) in devises"
                :key="devise"
                :value="index"
                >{{ index }} - {{ devise }}</ion-select-option
              >
            </ion-select>
          </ion-item></ion-col
        >
      </ion-row>
    </ion-grid>
    <ion-button
      @click="submitConversion"
      class="submit-btn"
      color="dark"
      shape="round"
      expand="block"
      >Convertir</ion-button
    >
  </form>
</template>

<script>
import {
  IonInput,
  IonItem,
  IonSelect,
  IonSelectOption,
  IonLabel,
  IonCol,
  IonRow,
  IonGrid,
  IonButton,
  toastController,
} from "@ionic/vue";
import axios from "axios";

export default {
  name: "Search",
  components: {
    IonInput,
    IonItem,
    IonSelect,
    IonSelectOption,
    IonLabel,
    IonCol,
    IonRow,
    IonGrid,
    IonButton,
  },
  data() {
    return {
      cancelAttr: "cancel-text",
      dataInputAmout: null,
      dataSelectEntry: "EUR",
      dataSelectExit: "USD",
      devises: {
        EUR: "Euro",
        USD: "USD",
      },
    };
  },
  mounted() {
    axios
      .get(
        `http://data.fixer.io/api/symbols?access_key=${process.env.VUE_APP_API_KEY}`
      )
      .then((response) => {
        const symbolsResult = response.data;
        this.devises = symbolsResult.symbols;
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
    async openToast(message) {
      const toast = await toastController.create({
        message: message,
        duration: 2000,
      });
      return toast.present();
    },
    submitConversion() {
      if (
        !this.dataInputAmout ||
        this.dataInputAmout.length <= 0 ||
        this.dataInputAmout <= 0
      ) {
        this.openToast("Vous devez saisir un montant supérieur à 0.");
        return;
      }
      if (
        this.dataInputAmout.includes(".") &&
        this.dataInputAmout.substr(this.dataInputAmout.indexOf(".") + 1)
          .length > 2
      ) {
        this.openToast("Vous devez saisir un montant avec maximum 2 décimale.");
        return;
      }
      if (this.dataSelectEntry === this.dataSelectExit) {
        this.openToast(
          "Vous devez choisir une devise différente de la première."
        );
        return;
      }
      axios
        .get(
          `http://data.fixer.io/api/latest?access_key=${process.env.VUE_APP_API_KEY}`
        )
        .then((response) => {
          const currencyResult = response.data;
          const entryValue = currencyResult.rates[this.dataSelectEntry];
          const exitValue = currencyResult.rates[this.dataSelectExit];
          const convertValue = this.getValueFromEntryCurrencyToExitCurrency(
            entryValue,
            this.dataInputAmout,
            exitValue
          );
          const sendResult = {
            result: convertValue,
            devise: this.devises[this.dataSelectExit],
          };
          this.$bus.emit("send-search", sendResult);
        })
        .catch((error) => {
          console.log(error);
        });
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
      const priceFormat = Intl.NumberFormat("fr-FR", {
        style: "currency",
        currency: this.dataSelectExit,
      }).format(result);

      return priceFormat;
    },
  },
};
</script>

<style scoped>
.label-amout {
  border-left: 1px solid #00000033;
  padding-left: 10px;
  margin-left: -10px;
}

.submit-btn {
  margin: 25px;
}

.title-selector {
  font-size: 12px;
  color: #fff;
}

.item-select-entry {
  background-color: rgb(72, 197, 72);
}

.input-amout {
  margin: 5px 25px;
  border: 0px;
  box-shadow: 0px 0px 10px #00000040;
}

ion-item {
  --inner-border-width: 0px;
  border: 1px solid #00000021;
  border-radius: 10px;
}

ion-item:active {
  background-color: black;
}

ion-select {
  --padding-left: 0px;
  text-align: left;
}
ion-select::part(icon) {
  display: none;
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

.col-selector {
  padding: 5px 20px;
  width: 50%;
}

.row-selector {
  background-image: url("../asset/arrow-right-solid.svg");
  background-repeat: no-repeat;
  background-position: center bottom 15px;
  background-size: 20px;
}
</style>