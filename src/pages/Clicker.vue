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
        <img id="picture" src="https://i.redd.it/v6886xzt36i01.png" height="20px"  class="img-fluid" @click="multiplyCookies" >
        <div class="button-reset">
        <button class="btn btn-outline-dark py-2" @click="resetCookies">Reset Cookies</button>
      </div>
    </div>
    </div>

    <div class="col">
      <div class="text-center">
      <div id="app">
      <div @click="incrementCounter" id="Cookie"></div>
      <h1 v-if="showBonusButton">MEGA BONUS!!!</h1>
      <p v-if="showBonusButton">(real expensive though)</p>
      <button v-if="showBonusButton" class="btn btn-outline-dark py-2" @click="activateBonus" :disabled="cookies < 906090000" id="bonus-button">
    Activate Bonus (Cost: 906090000 clicks)
  </button>
      <div v-if="bonusActive" id="bonus-timer">Bonus Active: {{ bonusTimer }}s</div>
      <img v-if="showBonusButton" src="https://media.tenor.com/wUv4TBeeuZoAAAAC/cat-huh.gif" alt="huh" class="huh">
      <p v-if="showBonusButton" class="huh1">GRIND MORE LMAO</p>
      <div class="music2">
      <audio v-if="showBonusButton" controls loop autoplay>
      <source src="@/audio/mustamies.mp3" type="audio/mp3">
    </audio>
      </div>
    </div>
  </div>
  <div class="music">
  <audio v-if="!showBonusButton" controls autoplay loop>
      <source src="@/audio/msc.mp3" type="audio/mp3">
    </audio>
</div>
    </div>

    <div class="col">
      <div class="bg-upgrade"
      style="background-image: url(https://orteil.dashnet.org/cookieclicker/img/bgPink.jpg);
      height: 100vh;">
      <h1 class="text-center">Store</h1>
      <div class="buttons">
      <button v-for="(upgrade, name) in upgrades"
        class="btn btn-outline-dark py-3"
        :disabled="cookies<upgrade.price"
        @click="buyUpgrade(upgrade)">
        <img src="https://www.iconpacks.net/icons/2/free-fast-buy-icon-3048-thumb.png" alt="buy" class="buy">
        Buy {{ name }} for 
        {{ upgrade.price }} clicks ({{upgrade.cps}} clicks per second)
        {{ upgrade.count }}
      </button>
    </div>
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
      computed: {
      showBonusButton() {
      return this.upgrades.grandma.count >= 10;
    },
  },

    data(){
        return {
            cookies: 0,
            shouldMultiplyCookies: false,
            upgrades: {
               cursor: {price: 5, cps: 1, count: 0},
               grandma: {price: 10, cps: 2, count: 0},
               farm: {price: 100, cps: 10, count: 0},
               factory: {price: 1000, cps: 100, count: 0},
               temple: {price: 10000, cps: 1000, count: 0},
               goldencursor: {price: 100000, cps: 1*70, count: 0},
            },
            bonusActive: false,
            bonusTimer: 0,
            bonusDuration: 5,
            cps: 100, 
        }
    },
    methods: {
        buyUpgrade(upgrade){
            if (this.cookies >= upgrade.price) {
                this.cookies -= upgrade.price;
                upgrade.price += Math.ceil(upgrade.price*0.25)         
                upgrade.count++;
            }
        },
      resetCookies() {
        this.cookies = 1;
        this.shouldMultiplyCookies = true;
      },

      multiplyCookies() {
      if (this.shouldMultiplyCookies) {
        this.cookies *= 2; 
        this.shouldMultiplyCookies = true;
      } else {
        this.cookies++;
      }
    },

      activateBonus() {
        const price = 906090000;
        const cps = 100;

      if (!this.bonusActive && this.cookies >= price) {
        this.cookies -= price;
        this.bonusActive = true;
        this.bonusTimer = this.bonusDuration;

        const timerInterval = setInterval(() => {
          this.bonusTimer--;

        if (this.bonusTimer <= 0) {
            clearInterval(timerInterval);
            this.bonusActive = false;
            } else {
            this.cookies += cps;
          }
        }, 1);
      }
    }
  },

}

</script>

<style>

.music2 {
  margin-top: 500px;
  opacity: 0%;
}

.music {
  margin-top: 630px;
  opacity: 0%;
}

.huh1 {
  font-size: 30px;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-style: italic;
}

.huh {
  margin-top: 30px;
  height: 400px;
}

.p {
  font-weight: 1;
}

.buy {
height: 30px;
}

.audiopic {
  height: 40px;
  margin-top: 20px;
}

.buttons {
 margin-left: 130px;
 margin-right: 100px;
 margin-top: 50px;
}


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