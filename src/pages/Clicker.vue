<template>
  <div class="row">
    <div class="col">
      <p class="text-center">Cookies {{ cookies }}</p>
      <img
        src="https://pngimg.com/d/cookie_PNG13656.png"
        class="img-fluid clickable-cookie"
        @click="handleClick"
      />
      <!-- Add audio element for the pop sound -->
      <audio ref="popSound" preload="auto" @loadeddata="audioLoaded">
        <source :src="popSound" type="audio/mpeg">
        Your browser does not support the audio element.
      </audio>
    </div>
    <div class="col">
      Column
    </div>
    <div class="col">
      <button
        v-for="(upgrade, name) in upgrades"
        class="btn btn-outline-primary py-3"
        :disabled="cookies < upgrade.price"
        @click="buyUpgrade(upgrade)"
      >
        Buy {{ name }} for
        {{ upgrade.price }} clicks ({{ upgrade.cps }} clicks per second)
        {{ upgrade.count }}
      </button>
    </div>
  </div>
</template>

<script>
import popSoundFile from './pop-sound.mp3';

export default {
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
      popSound: popSoundFile,
    };
  },
  methods: {
    handleClick() {
      const clickableCookie = document.querySelector(".clickable-cookie");
      clickableCookie.classList.add("clicked");

      // Check if the audio is fully loaded before playing
      if (this.$refs.popSound.readyState >= 2) {
        this.$refs.popSound.play();
      }

      setTimeout(() => {
        clickableCookie.classList.remove("clicked");
      }, 200);

      this.cookies++;
    },
    buyUpgrade(upgrade) {
      if (this.cookies >= upgrade.price) {
        this.cookies -= upgrade.price;
        upgrade.price += Math.ceil(upgrade.price * 0.25);
        upgrade.count++;
      }
    },
    audioLoaded() {
      // Optionally, you can handle logic when the audio is loaded
      console.log('Audio is loaded.');
    },
  },
};
</script>

<style scoped>
.clickable-cookie {
  cursor: pointer;
  transition: transform 0.2s ease-in-out;
}

.clicked {
  transform: scale(1.05);
}
</style>
