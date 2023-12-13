<template>
  <div class="pink">
    <div class="row">
      <div class="col">
        <p class="text-center">Cookies {{ cookies }}</p>
        <!-- Südame animatsioonikomponent -->
        <HeartAnimation :showHearts="showHearts" />
        <!-- Cookie pilt ja sündmus, mis käivitab südame animatsiooni -->
        <img src="https://pngimg.com/d/cookie_PNG13656.png" class="img-fluid" @click="showHearts = true; incrementCookies()">
      </div>
      <div class="col">
        Column
      </div>
      <div class="col">
        <button v-for="(upgrade, name) in upgrades"
          class="btn btn-outline-primary py-3"
          :disabled="cookies < upgrade.price"
          @click="buyUpgrade(upgrade)">
          Buy {{ name }} for 
          {{ upgrade.price }} clicks ({{ upgrade.cps }} clicks per second)
          {{ upgrade.count }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import HeartAnimation from './HeartAnimation.vue';

export default {
  components: {
    HeartAnimation,
  },
  data() {
    return {
      cookies: 0,
      showHearts: false,
      upgrades: {
        cursor: { price: 10, cps: 0.1, count: 0 },
        grandma: { price: 100, cps: 1, count: 0 },
        farm: { price: 1000, cps: 10, count: 0 },
        factory: { price: 10000, cps: 100, count: 0 },
        temple: { price: 100000, cps: 1000, count: 0 },
      },
    };
  },
  methods: {
    buyUpgrade(upgrade) {
      if (this.cookies >= upgrade.price) {
        this.cookies -= upgrade.price;
        upgrade.price += Math.ceil(upgrade.price * 0.25);
        upgrade.count++;
      }
    },
    incrementCookies() {
      this.cookies++;
    },
  },
};
</script>

<style>
.pink{
  background-color: pink;
  width: auto;
  height: 600000px;
}
</style>
