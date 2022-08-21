<template>

	<ion-list>
		<ion-item v-for="i in history" :key="i.date">
			<ion-label> {{ i.date }} </ion-label>
			<ion-label> ${{ i.value }} </ion-label>
    </ion-item>
	</ion-list>

</template>

<script lang="ts">
import axios from 'axios';

export default {
  name: 'CryptoHistory',
  props: ['cryptoId'],

  cryptoId: '',
  history: [],
  
  data (): object {
    return {
      cryptos: this.cryptoId,
      history: this.history,
    };
  },

  created () {
    let cryptoId = this.cryptoId;

    if (cryptoId !== undefined) {
      axios
        .get(`https://api.coincap.io/v2/assets/${cryptoId}/history?interval=d1`)
        .then((res) => {
          
          this.history = res.data.data.map((i: { priceUsd: number; date: string; }) => {
            return {
              value: Number(i.priceUsd).toFixed(2),
              date: i.date
            } 
          }).reverse();
          
      });
    }

  }
}
</script>

<style>

</style>
