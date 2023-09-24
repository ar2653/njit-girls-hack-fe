<template>
  <template v-if="showLandingPage">
    <div class="space-trip">
      <div class="text-box">
        <div class="heading">Make my space trip!</div>
        <div class="button-wrapper">
          <div class="button">Learn More</div>
          <div class="button" @click="changeHomePage">Book Trip</div>
        </div>
      </div>
    </div>
  </template>
  <template v-else>
    <h1>MAKE MY SPACE TRIP</h1>
    <div class="button-wrapper-1">
        <div class="button" @click="changeBack">Go Back</div>

    </div>

    <Scene @onSceneLoad="onSceneLoad" />
    <div ref="loading" class="loading-screen">
      <div class="planet">
        <div class="cloud"></div>
        <div class="cloud"></div>
        <div class="cloud"></div>
      </div>
      <p>Loading...</p>
    </div>
  </template>
</template>

<script>
    import Scene from "./components/Scene.vue";
    export default {
        components: {
            Scene,
},
        data(){
            return {
                showLandingPage:true
            }
        },
        methods: {
            onSceneLoad() {
                this.$refs.loading.style.display = "none";
            },
            changeHomePage() {
                this.showLandingPage = false;
            },
            changeBack() {
                this.showLandingPage = true;
            }

        }
    }
</script>

<style scoped lang="scss">
.loading-screen {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  width: 100%;
  height: 100%;
  background-color: var(--dark);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 16px;
  font-weight: bold;
  .planet {
    width: 80px;
    height: 80px;
    overflow: hidden;
    border-radius: 50%;
    background-color: #3009e0;
    position: relative;
    box-shadow: inset -20px 0px 12px 0px #05014d;
    .cloud {
      border-radius: var(--radius);
      background-color: rgb(208, 208, 208);
      width: 30px;
      height: 10px;
      position: absolute;
      transform: translateY(-100px);
    }
    .cloud:nth-child(1) {
      top: 3px;
      animation: cloud 1500ms linear 100ms infinite;
    }
    .cloud:nth-child(2) {
      top: 28px;
      animation: cloud 1700ms linear 50ms infinite;
    }
    .cloud:nth-child(3) {
      top: 58px;
      animation: cloud 1800ms linear 200ms infinite;
    }
  }
}
@keyframes cloud {
  0% {
    transform: translateX(-50px);
  }
  100% {
    transform: translateX(90px);
  }
}

    .space-trip {
  width: 100vw;
  height: 100vh;
  margin: 0;
  background: black;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative; /* Added for z-index */
}



.text-box {
  font-size: 60px;
  font-family: "Orbitron", sans-serif;
  letter-spacing: 7px;
  word-spacing: 12px;
  text-transform: uppercase;
  color: rgb(238, 238, 238);
}

.button-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  flex-direction: row;
}


.button-wrapper-1 {
  display: flex;
  justify-content: left;
  align-items: left;
  padding-top: 50px;
  flex-direction: row;
}
.button {
  display: inline-block;
  font-size: 16px;
  font-weight: bold;
  padding: 15px;
  margin-left: 20px;
  margin-right: 20px;
  word-spacing: normal;
  letter-spacing: 5px;
  border: 1px solid white;
  background: black;
  cursor: pointer;
  color: white; /* Added text color */
  text-decoration: none; /* Remove underline for buttons */
  transition: background-color 0.3s, color 0.3s; /* Smooth transition */
}

.button:hover {
  background-color: white;
  color: black;
}
</style>
