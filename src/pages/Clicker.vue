<template>
  <div class="row" :style="{ backgroundColor: backgroundColor }">
    <div class="col">
      <p class="text-center">Cookies {{ cookies }}</p>
      <img src="https://pngimg.com/d/cookie_PNG13656.png" class="img-fluid" @click="cookies++">
    </div>
    <div class="col">
      Column
    </div>
    <div class="col">
      <button v-for="(upgrade, name) in upgrades"
        class="btn btn-outline-primary py-3"
        :disabled="cookies<upgrade.price"
        @click="buyUpgrade(upgrade, name)">
        Buy {{ name }} for 
        {{ upgrade.price }} clicks ({{upgrade.cps}} clicks per second)
        {{ upgrade.count }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  created() {
    setInterval(() => {
      for (const upgrade in this.upgrades) {
        this.cookies = (
          parseFloat(this.cookies) + this.upgrades[upgrade].cps * this.upgrades[upgrade].count
        ).toFixed(1);
      }
    }, 1000);
  },
  data() {
    return {
      cookies: 0,
      backgroundColor: 'white',
      upgrades: {
        cursor: { price: 10, cps: 0.1, count: 0 },
        grandma: { price: 100, cps: 1, count: 0 },
        farm: { price: 1000, cps: 10, count: 0 },
        factory: { price: 10000, cps: 100, count: 0 },
        temple: { price: 100000, cps: 1000, count: 0 },
        colorChange: { price: 15, cps: 0, count: 0 },
      },
    };
  },
  methods: {
    buyUpgrade(upgrade, name) {
      if (this.cookies >= upgrade.price) {
        this.cookies -= upgrade.price;
        upgrade.price += Math.ceil(upgrade.price * 0.25);
        upgrade.count++;

        if (name === 'colorChange') {
          this.backgroundColor = this.getRandomColor();
        }
      }
    },

    getRandomColor() {
      const letters = '0123456789ABCDEF';
      let color = '#';
      for (let i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    },
  },
};
</script>
