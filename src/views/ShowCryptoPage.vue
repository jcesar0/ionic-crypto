<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>{{ crypto.name }}</ion-title>
      </ion-toolbar>
    </ion-header>
    
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">{{ crypto.name }}</ion-title>
        </ion-toolbar>
      </ion-header>
    
      <div id="container">
        <CryptoHistory v-bind:cryptoId="slug"></CryptoHistory>
      </div>


    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar } from '@ionic/vue';
import { defineComponent } from 'vue';
import { useRoute } from 'vue-router';

import axios from 'axios';
import CryptoHistory from '@/components/CryptoHistory.vue';

export default defineComponent({
  crypto: {
    name: '',
    id: '',
  },
  
  name: 'ShowCryptoPage',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    CryptoHistory
  },

  data () {
    return {
      crypto: this.getCryptoData(this.slug),
    };
  },

  methods: {
    async getCryptoData(crypto: string) {
      await axios
        .get(`https://api.coincap.io/v2/assets/${crypto}`)
        .then((res) => {
          this.crypto = res.data.data;
      });
    }

  },

  setup () {
    const route = useRoute();
    const { slug } = route.params;
    return { slug };
  },
});
</script>

<style scoped>
</style>
