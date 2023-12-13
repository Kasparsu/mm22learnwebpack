<template>
  <div class="row">
    <div class="col">
      <p class="text-center">Cookies {{ cookies }}</p>
      <img src="https://pngimg.com/d/cookie_PNG13656.png" class="img-fluid" @click="clickCookie">
      <audio ref="audio" src="./boom soundS.mp3"></audio>
    </div>
    <div class="col">
      Column
    </div>
    <div class="col">
      <!-- Your existing button code -->
      <!-- ... -->
    </div>
  </div>
</template>

<script>
export default {
  created() {
    setInterval(() => {
      for (const upgrade in this.upgrades) {
        this.cookies = (
          parseFloat(this.cookies) +
          this.upgrades[upgrade].cps * this.upgrades[upgrade].count
        ).toFixed(1);
      }
    }, 1000);
  },
  data() {
    return {
      cookies: 0,
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
    clickCookie() {
      // Play the click sound when the cookie is clicked
      this.$refs.audio.play();
      // Increment the cookie count
      this.cookies++;
    },
    buyUpgrade(upgrade) {
      if (this.cookies >= upgrade.price) {
        this.cookies -= upgrade.price;
        upgrade.price += Math.ceil(upgrade.price * 0.25);
        upgrade.count++;
      }
    },
  },
};
</script>