<template>
  <div class="row">
    <div class="col">
      <p class="text-center">Cookies {{ cookies }}</p>
      <img src="https://pngimg.com/d/cookie_PNG13656.png" class="img-fluid" @click="cookies++">
    </div>
    <div class="col">
      Column
    </div>
    <div class="col">
      <button v-for="(upgrade, name) in upgrades"
        :key="name"
        class="btn btn-outline-dark py-3"
        :disabled="cookies < upgrade.price"
        @click="buyUpgrade(upgrade)">
        Buy {{ name }} for 
        {{ upgrade.price }} clicks ({{upgrade.cps}} clicks per second)
        {{ upgrade.count }}
      </button>
      <button class="btn btn-outline-success py-3" @click="toggleAutoBuy">
        {{ autoBuyEnabled ? 'Turn Off Auto-Buy' : 'Turn On Auto-Buy' }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cookies: 0,
      autoBuyEnabled: false,
      upgrades: {
        cursor: { price: 10, cps: 0.1, count: 0 },
        grandma: { price: 100, cps: 1, count: 0 },
        farm: { price: 1000, cps: 10, count: 0 },
        factory: { price: 10000, cps: 100, count: 0 },
        temple: { price: 100000, cps: 1000, count: 0 },
        Goverment: { price: 150000, cps: 10000, count: 0 },
        moon: { price: 1000000, cps: 100000, count: 0 },
      },
    };
  },
  created() {
    this.startAutoBuy();
  },
  methods: {
    buyUpgrade(upgrade) {
      if (this.cookies >= upgrade.price) {
        this.cookies -= upgrade.price;
        upgrade.price += Math.ceil(upgrade.price * 0.25);
        upgrade.count++;
      }
    },
    toggleAutoBuy() {
      this.autoBuyEnabled = !this.autoBuyEnabled;
      if (this.autoBuyEnabled) {
        this.startAutoBuy();
      }
    },
    startAutoBuy() {
      setInterval(() => {
        for (const upgrade in this.upgrades) {
          this.cookies = (
            parseFloat(this.cookies) + this.upgrades[upgrade].cps * this.upgrades[upgrade].count
          ).toFixed(1);
        }

        if (this.autoBuyEnabled) {
          this.autoBuyUpgrade();
        }
      }, 1000);
    },
    autoBuyUpgrade() {
      for (const upgrade in this.upgrades) {
        const currentUpgrade = this.upgrades[upgrade];
        if (this.cookies >= currentUpgrade.price) {
          this.cookies -= currentUpgrade.price;
          currentUpgrade.price += Math.ceil(currentUpgrade.price * 0.25);
          currentUpgrade.count++;
          break;
        }
      }
    },
  },
};
</script>