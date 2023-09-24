<template>
  <template v-if="showLandingPage">
    <div class="space-trip">
      <div class="typewriter">
        <h1>
          <p>
            AI is everywhere, from your smart fridge to your selfie filters.
          </p>
          <p>
            But amidst this, SPACE TRAVEL still stands as a beacon of human
            ingenuity,
          </p>
          <p>reminding us of the grandeur of exploration beyond our planet.</p>
        </h1>
      </div>

      <div class="text-box">
        <div class="heading">Make my space trip!</div>
        <div class="button-wrapper">
          <div class="button" @click="showTechStack">TECH STACK</div>
          <div class="button" @click="changeHomePage">Book My Trip</div>
          <div class="button" @click="showMeetTheTeam">MEET the TEAM</div>
          <div class="button" @click="redirectToGithub">Know more details</div>
        </div>
        <div v-if="techStack" class="tech-stack">
          <h1>WEBGL, THREEJS, VUE, VITE, NODEJS, EXPRESS, MONGODB, GOOGLE CLOUD</h1>
        </div>
        <div v-if="meetTheTeam" class="meet-team">
          <h1>TABREZ__(FRONTEND), ANKUSH___(BACKEND), RICHA____(DATABASE)</h1>
        </div>
      </div>
    </div>
  </template>
  <template v-else>
    <h1>MAKE MY SPACE TRIP !</h1>
    <Scene @onSceneLoad="onSceneLoad" />
    <div ref="loading" class="loading-screen">
      <div class="planet">
        <div class="cloud"></div>
        <div class="cloud"></div>
        <div class="cloud"></div>
      </div>
      <p>Loading . . .</p>
    </div>
  </template>
</template>

<script>
import Scene from "./components/Scene.vue";
export default {
  components: {
    Scene,
  },
  data() {
    return {
      showLandingPage: true,
      techStack: false,
      meetTheTeam: false,
    };
  },
  methods: {
    onSceneLoad() {
      this.$refs.loading.style.display = "none";
    },
    changeHomePage() {
      this.showLandingPage = false;
    },
    showTechStack() {
      this.techStack = true;
      this.closeAfterDelay(10000);
    },
    showMeetTheTeam() {
      this.meetTheTeam = true;
      this.closeAfterDelay(8000);
    },
    redirectToGithub() {
      window.open("https://github.com/tabrezdn1/njit-girls-hack#readme", "_blank");
    },
    closeAfterDelay(time) {
      setTimeout(() => {
        this.techStack = false;
        this.meetTheTeam = false;
      }, time);
    },
  },
};
</script>

<style scoped lang="scss">
/* DEMO-SPECIFIC STYLES */
.tech-stack {
  position: absolute;
  top: 660px;
  width: 200px;
}
.meet-team {
  position: absolute;
  top: 660px;
  right: 800px;
}
.heading{
  padding: 30px;
}
.typewriter h1 {
  top: 150px;

  overflow: hidden; /* Ensures the content is not revealed until the animation */
  border-right: 0.15em solid orange; /* The typwriter cursor */
  white-space: nowrap; /* Keeps the content on a single line */
  margin: auto; /* Gives that scrolling effect as the typing happens */
  letter-spacing: 0.15em; /* Adjust as needed */
  animation: typing 3.5s steps(40, end), blink-caret 0.75s step-end infinite;
}

/* The typing effect */
@keyframes typing {
  from {
    width: 0;
  }
  to {
    width: 50%;
  }
}

/* The typewriter cursor effect */
@keyframes blink-caret {
  from,
  to {
    border-color: transparent;
  }
  50% {
    border-color: orange;
  }
}
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

h1 {
  font-family: "Orbitron", sans-serif;
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
