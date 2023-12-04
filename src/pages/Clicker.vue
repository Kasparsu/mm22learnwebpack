<template>
  	<div class="bg-image" 
     style="background-image: url('https://orteil.dashnet.org/cookieclicker/img/bgCandy.jpg');
            height: 100vh">
      <div class="row">
    <div class="col">
      <div class="cookie-bg"
      style="background-image: url(https://orteil.dashnet.org/cookieclicker/img/bgPink.jpg);
      height: 100vh;">
      <h1 class="text-center">Faya's Bakery</h1>
      <h2 id="cookies" class="text-center">Cookies {{ cookies }}</h2>
        <img id="picture" src="https://i.redd.it/v6886xzt36i01.png" height="20px"  class="img-fluid" @click="cookies++" >
        <div class="button-reset">
        <button class="btn btn-outline-dark py-2" @click="resetCookies">Reset Cookies</button>
      </div>
    </div>
    </div>
    <div class="col">
    </div>
    <div class="col">
      <div class="bg-upgrade"
      style="background-image: url(https://orteil.dashnet.org/cookieclicker/img/bgPink.jpg);
      height: 100vh;">
      <h1 class="text-center">Store</h1>
      <button v-for="(upgrade, name) in upgrades"
        class="btn btn-outline-dark py-3"
        :disabled="cookies<upgrade.price"
        @click="buyUpgrade(upgrade)">
        Buy {{ name }} for 
        {{ upgrade.price }} clicks ({{upgrade.cps}} clicks per second)
        {{ upgrade.count }}
      </button>
    </div>
  </div>
</div>
	</div>
</template>

<script>
export default {
    created(){
        setInterval(()=> {
            for(const upgrade in this.upgrades){
                this.cookies = (
                    parseFloat(this.cookies) + this.upgrades[upgrade].cps * this.upgrades[upgrade].count
                ).toFixed(1);
            }
        }, 1000);
    },
    data(){
        return {
            cookies: 0,
            upgrades: {
               cursor: {price: 10, cps: 0.1, count: 0},
               grandma: {price: 100, cps: 1, count: 0},
               farm: {price: 1000, cps: 10, count: 0},
               factory: {price: 10000, cps: 100, count: 0},
               temple: {price: 100000, cps: 1000, count: 0},
               GoldenCursor: {price: 10000000000, cps: 1000000000000000, count: 0},
            }
        }
    },
    methods: {
        buyUpgrade(upgrade){
            if(this.cookies>=upgrade.price){
                this.cookies-=upgrade.price;
                upgrade.price += Math.ceil(upgrade.price*0.25)         
                upgrade.count++;
            }
        },
      resetCookies() {
        this.resetCookies
      }
    }
}
</script>

<style>

.button-reset{
  display: flex;
  justify-content: center;
  align-items: center;
  height: auto;
}

.img-fluid {
  animation: bouncing 10s infinite;
}

@keyframes bouncing {
  0%, 2%, 4%, 6%, 8% {
    transform: translateY(0);
  }

  1% {
    transform: translateY(-15);
  }

  3% {
    transform: translateY(-10px);
  }

  5% {
    transform: translateY(-5px);
  }

  7% {
    transform: translateY(-2px);
  }
}

</style>