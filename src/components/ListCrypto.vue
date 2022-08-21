<template>

	<ion-list>
		<ion-item v-for="crypto in cryptos" :key="crypto.slug" @click="() => router.push(`/crypto/${crypto.id}`)">
			<ion-label class="crypto-list-item"> {{ crypto.name }} <span> {{ crypto.slug }} </span> </ion-label>
			<ion-label class="crypto-list-price" v-bind:class="crypto.down ? 'down' : 'up'"> ${{ crypto.value }} </ion-label>
    </ion-item>
	</ion-list>

</template>

<script lang="ts">
	import axios from 'axios';
  import router from '../router';
  import { useRouter } from 'vue-router';

	export default {
    name: 'ListCrypto',
    cryptos: [],

		data () {
			return {
				cryptos: this.cryptos
			};
		},

		mounted() {
			axios
				.get('https://api.coincap.io/v2/assets')
				.then((res) => {
					
          this.cryptos =
						res.data.data.map((i: {
              id: string, 
              name: string; 
              symbol: string; 
              priceUsd: number; 
              changePercent24Hr: number; 
            }) => {
							return {
                id: i.id,
								name: i.name,
								slug: i.symbol,
								value: Number(i.priceUsd).toFixed(2),
								down: i.changePercent24Hr > 0,
							}
						});

      });
		},


    setup() {
      const router = useRouter();
      return { router };
    },
	}
</script>

<style>
	.crypto-list-item {
		font-weight: 700;
		font-size: 14px;
	}

	.crypto-list-item span {
		font-weight: 500;
		color: #b9b4b4;
	}

	.crypto-list-price {
		font-weight: 700;
	}

	.crypto-list-price.up {
		color: #16c784 !important;
	}

	.crypto-list-price.down {
		color: #ea3943 !important;
	}

	.crypto-list-avatar {
		margin-right: 20px;
	}
</style>
