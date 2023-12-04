<template>
  <div class="row">
    <div class="col">
      <p class="text-center">Cookies {{ cookies }}</p>
      <div class="circle-container">
  <div class="circle" v-for="(upgrade, index) in cursorUpgrades" :key="index" :style="{ transform: `rotate(${index * 40}deg) translate(70px) rotate(-${index * 5}deg)` }"></div>
  <img src="https://pngimg.com/d/cookie_PNG13656.png" class="img-fluid central-circle" @click="cookies++">
</div>
      
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
            cursorUpgrades: [],
            upgrades: {
               cursor: {price: 10, cps: 0.1, count: 0},
               grandma: {price: 100, cps: 1, count: 0},
               farm: {price: 1000, cps: 10, count: 0},
               factory: {price: 10000, cps: 100, count: 0},
               temple: {price: 100000, cps: 1000, count: 0},
            }
        }
    },
    methods: {
    buyUpgrade(upgrade, name){
        if(this.cookies>=upgrade.price){
            this.cookies-=upgrade.price;
            upgrade.price += Math.ceil(upgrade.price*0.25)         
            upgrade.count++;
            if(name === 'cursor') {
                this.cursorUpgrades.push({});
            }
        }
    }
}
}
</script>
<style scoped>
.circle-container {
  position: relative;
  width: 200px;
  height: 200px;
  margin: 0 auto;
}

.circle {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 50px;
  height: 50px;
  margin: -25px 0 0 -25px;
  background-color: #555;
  border-radius: 50%;
}

.central-circle {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 100px;
  height: 100px;
  margin: -50px 0 0 -50px;
  border-radius: 50%;
}
img {
  margin: 150px;
height: 200px;
width: 250px;
}
</style>