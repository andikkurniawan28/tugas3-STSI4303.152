<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Cryptocurrency Market</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <div id="container">
        <!-- Tombol Refresh -->
        <p>
          <ion-button @click="ambilDataCrypto">Refresh</ion-button>
        </p>

        <!-- Daftar Cryptocurrency -->
        <div class="crypto-container">
          <div v-for="coin in cryptoData" :key="coin.id" class="crypto-card">
            <div class="crypto-row">
              <div class="col-rank">
                <span class="small-text">Rank</span><br>
                <strong>{{ coin.rank }}</strong>
              </div>
              <div class="col-name">
                <span class="small-text">{{ coin.name }}</span><br>
                <strong>{{ coin.symbol }}</strong>
              </div>
              <div class="col-price">
                <span class="small-text">USD</span><br>
                <strong>{{ coin.price_usd }}</strong>
              </div>
            </div>
          </div>
        </div>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonButton } from '@ionic/vue';
import { defineComponent } from 'vue';
import EndPointAccess from '@/services/EndPointAccess';

export default defineComponent({
  name: 'Home',
  data() {
    return {
      cryptoData: [] as any[]
    }
  },
  mounted() {
    // Mengambil data secara otomatis saat halaman dibuka
    this.ambilDataCrypto();
  },
  methods: {
    ambilDataCrypto() {
      const apiURL = 'https://api.coinlore.net/api/tickers/';
      const cryptoService = new EndPointAccess(apiURL);

      cryptoService.getRes()
        .then((response: any) => {
          // Data dari API Coinlore tersimpan di dalam array response.data.data
          this.cryptoData = response.data.data;
        })
        .catch((error) => {
          console.error("Gagal mengambil data crypto:", error);
        });
    }
  },
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonButton
  }
});
</script>

<style scoped>
#container {
  padding: 16px;
  text-align: center;
}

.crypto-container {
  max-width: 450px;
  margin: 0 auto;
}

.crypto-card {
  background: #fdfaf6;
  border: 1px solid #e2dac9;
  border-radius: 6px;
  padding: 10px 14px;
  margin-bottom: 8px;
}

.crypto-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  text-align: left;
}

.small-text {
  font-size: 11px;
  color: #666;
}

.col-rank {
  width: 20%;
}

.col-name {
  width: 50%;
}

.col-price {
  width: 30%;
  text-align: right;
}
</style>
