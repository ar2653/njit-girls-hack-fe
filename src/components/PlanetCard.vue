<template>
  <div class="planet-card">
    <img
      class="planet-img"
      :src="`./assets/cards/${planetInfo.name}.png`"
      :alt="planetInfo.displayName"
    />
    <div class="title">
      <h2>{{ planetInfo.displayName }}</h2>
    </div>
    <button class="close" @click="this.$emit('closeCard')">×</button>
    <div class="temperature">
      <p v-if="planetInfo.meanTemp" title="Surface temperature">
        mean:
        <span
          class="value"
          :style="{ color: temperatureColor(planetInfo.meanTemp) }"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            :stroke="temperatureColor(planetInfo.meanTemp)"
            class="icon icon-tabler icon-tabler-temperature"
            width="22"
            height="22"
            viewBox="0 0 24 24"
            stroke-width="2"
            fill="none"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path stroke="none" d="M0 0h24v24H0z" fill="none" />
            <path d="M10 13.5a4 4 0 1 0 4 0v-8.5a2 2 0 0 0 -4 0v8.5" />
            <line x1="10" y1="9" x2="14" y2="9" />
          </svg>
          {{ planetInfo.meanTemp }} °C
        </span>
      </p>
      <p v-if="planetInfo.minTemp" title="Surface temperature">
        min:
        <span
          class="value"
          :style="{ color: temperatureColor(planetInfo.minTemp) }"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            :stroke="temperatureColor(planetInfo.minTemp)"
            class="icon icon-tabler icon-tabler-temperature"
            width="22"
            height="22"
            viewBox="0 0 24 24"
            stroke-width="2"
            fill="none"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path stroke="none" d="M0 0h24v24H0z" fill="none" />
            <path d="M10 13.5a4 4 0 1 0 4 0v-8.5a2 2 0 0 0 -4 0v8.5" />
            <line x1="10" y1="9" x2="14" y2="9" />
          </svg>
          {{ planetInfo.minTemp }} °C
        </span>
      </p>
      <p v-if="planetInfo.maxTemp" title="Surface temperature">
        max:
        <span
          class="value"
          :style="{ color: temperatureColor(planetInfo.maxTemp) }"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            :stroke="temperatureColor(planetInfo.maxTemp)"
            class="icon icon-tabler icon-tabler-temperature"
            width="22"
            height="22"
            viewBox="0 0 24 24"
            stroke-width="2"
            fill="none"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path stroke="none" d="M0 0h24v24H0z" fill="none" />
            <path d="M10 13.5a4 4 0 1 0 4 0v-8.5a2 2 0 0 0 -4 0v8.5" />
            <line x1="10" y1="9" x2="14" y2="9" />
          </svg>
          {{ planetInfo.maxTemp }} °C
        </span>
      </p>
    </div>
    <div class="info">
      <h5>{{ planetInfo.caption }}</h5>
      <p class="description">{{ planetInfo.description }}</p>
      <h5>BASE PRICE: {{ planetInfo.price }} ETH</h5>
      <ul>
        <li>
          Radius: <span class="value">{{ planetInfo.radius }} km</span>
        </li>
        <li v-if="planetInfo.timesLarger && planetInfo.timesLarger !== -1">
          <span v-if="planetInfo.timesLarger >= 1"
            ><span class="value">{{ planetInfo.timesLarger }}x</span> larger
            than Earth</span
          >
          <span v-else
            ><span class="value"
              >{{ (1 / planetInfo.timesLarger).toFixed(2) }}x</span
            >
            smaller than Earth</span
          >
        </li>
        <li
          v-if="planetInfo.distanceFromSun && planetInfo.distanceFromSun !== -1"
        >
          Distance from Sun:
          <span class="value">{{ planetInfo.distanceFromSun }} AU</span>
        </li>
        <li v-if="planetInfo.distance">
          Distance from
          {{
            planetInfo.orbitObject.charAt(0).toUpperCase() +
            planetInfo.orbitObject.slice(1)
          }}: <span class="value">{{ planetInfo.distance }} km</span>
        </li>
        <li v-if="planetInfo.year">
          1 year: <span class="value">{{ planetInfo.year }}</span>
        </li>
        <li v-if="planetInfo.day">
          1 day: <span class="value">{{ planetInfo.day }}</span>
        </li>
        <li v-if="planetInfo.moons != null">
          Moons: <span class="value">{{ planetInfo.moons }}</span>
        </li>
      </ul>
      <div>
        <h5>Fill in your details!</h5>
        <form>
            <label for="fname">First name:</label><br>
            <input type="text" id="fname" required name="fname" v-model="user.first_name"><br>
            <label for="lname">Last name:</label><br>
            <input type="text" id="lname" required name="lname" v-model="user.last_name"><br>
            <label for="email_address">Email address</label><br>
            <input type="text" id="email_address" required name="email_address" v-model="user.email_address"><br>
            <label for="cosmic_name">Cosmic handle</label><br>
            <input type="text" id="cosmicname" required name="cosmicname" v-model="user.cosmic_handle"><br>
            <label for="tripDate">Trip Date</label><br>
            <input type="date" id="tripdate" required name="tripdate" v-model="user.trip_date"><br><br>

            <label for="cars">Choose Resources</label> <br>
            
            <input type="radio" id="basic" name="basic" value="Basic" v-model="resource_package">
            <label for="basic" class="tooltip">Basic
              <span class="tooltiptext">20% more than base price</span>
            </label><br>
            <input type="radio" id="premium" name="premium" value="Premium" v-model="resource_package">
            <label for="basic" class="tooltip">Premium
              <span class="tooltiptext">30% more than base price</span>
            </label><br>
            <input type="radio" id="luxury" name="luxury" value="Luxury" v-model="resource_package">
            <label for="luxury" class="tooltip">Luxury
              <span class="tooltiptext">40% more than base price</span>
            </label><br><br>
            <h5 @click="bookAppointment(planetInfo)" class="book-trip">Book</h5>


        </form> 
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: {
    planetInfo: Object,
  },
  data() {
    return {
      resource_package: "",
      user: {first_name: "", last_name:"", email_address:"", cosmic_handle: "", trip_date: ""},
      temperatureColors: {
        "-130": "#a8f1ff",
        "-90": "#1fb6d0",
        "-40": "#c4f987",
        10: "#c4f987",
        50: "#ffcc33",
        90: "#ee6600",
        150: "#990000",
      },
      planetName: ""
    };
  },
  emits: ["closeCard"],
  methods: {
    bookAppointment(planet) {
      console.log(planet.name, this.resource_package);
      axios
        .post("https://mmstbe.ankushranapure.com/appointment/book", {
          first_name: this.user.first_name,
          last_name: this.user.last_name,
          cosmic_handle: this.user.cosmic_handle,
          displayName: planet.displayName,
          planet_image: planet.name,
          appointment_date: this.user.trip_date,
          package: this.resource_package
        })
        .then((response) => {
          console.log(response.data);
          this.resource_package = "";
          this.user = {first_name: "", last_name:"", email_address:"", cosmic_handle: "", trip_date: ""};
          this.$emit("closeCard");
        })
        .catch((error) => {
          console.log(error);
        });
    },
    temperatureColor(temp) {
      const keys = Object.keys(this.temperatureColors).map((t) => parseInt(t));
      keys.sort((a, b) => +a - +b);
      for (let key of keys) {
        if (key > temp) {
          return this.temperatureColors[key];
        }
      }
      return this.temperatureColors[keys[keys.length - 1]];
    }
  },
};
</script>

<style scoped lang="scss">
.book-trip {
  margin-left: 60px;
  background-color: #fff; /* Green */
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  cursor: pointer;
}
.tooltip {
  position: relative;
  display: inline-block;
  border-bottom: 1px dotted black;
}

.tooltip .tooltiptext {
  visibility: hidden;
  width: 120px;
  background-color: black;
  color: #fff;
  text-align: center;
  border-radius: 6px;
  padding: 5px 0;

  /* Position the tooltip */
  position: absolute;
  z-index: 1;
}

.tooltip:hover .tooltiptext {
  visibility: visible;
}
.planet-card {
  position: absolute;
  left: 15px;
  top: 50%;
  transform: translateY(-50%);
  width: 280px;
  border-radius: var(--radius);
  box-shadow: -8px -9px 14px rgb(255 255 255 / 8%);
  overflow: hidden;
  font-size: 14px;
  .planet-img {
    width: 100%;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    z-index: -1;
    background-color: var(--secondary);
  }
  .info {
    margin-top: 150px;
    min-height: 250px;
    width: 100%;
    border-radius: var(--radius);
    background: linear-gradient(
      131.76deg,
      var(--primary) -34.78%,
      var(--dark) 93.37%
    );
    padding: 6px 12px;
    h5 {
      text-align: center;
      font-size: 18px;
      color: var(--tertiary);
      font-weight: 100;
    }
    ul {
      margin: 10px 0;
      text-align: left;
      list-style: none;
      padding-left: 0;
      li {
        margin: 4px 0;
        color: #d5d5d5;
        .value {
          font-weight: bold;
          color: #fff;
        }
      }
    }
  }
  .title {
    position: absolute;
    text-align: center;
    top: 2px;
    width: 100%;
    h2 {
      text-transform: uppercase;
      font-weight: 400;
      font-size: 20px;
    }
  }
  .temperature {
    position: absolute;
    top: 50px;
    right: 10px;
    font-weight: 600;
    text-shadow: -4px 1px 11px #000;
    font-size: 14px;
    font-weight: lighter;
    .value {
      font-weight: bold;
      text-shadow: 0 0 10px #fff;
    }
    .icon {
      vertical-align: middle;
    }
  }
  .close {
    position: absolute;
    top: 2px;
    right: 6px;
    background-color: transparent;
    border: 0;
    color: #fff;
    font-size: 24px;
    cursor: pointer;
  }
  .description {
    margin: 6px 0;
  }
}
@media (max-width: 560px) {
  .planet-card {
    width: auto;
    height: auto;
    transform: none;
    top: 125px;
    left: 10px;
    right: 10px;
    bottom: 20px;
    z-index: 4;
    .info {
      top: 250px;
    }
    .close {
      font-size: 32px;
    }
  }
}
@media (max-height: 360px) {
  .planet-card {
    width: auto;
    height: auto;
    transform: none;
    top: 15px;
    left: 10px;
    right: 10px;
    bottom: 10px;
    z-index: 4;
    .planet-img {
      height: 100%;
      width: auto;
    }
    .info {
      top: 0;
      right: 0;
      width: auto;
      height: 100%;
      left: 230px;
    }
    .title {
      max-width: 250px;
    }
    .temperature {
      left: 110px;
      right: auto;
    }
    .close {
      right: auto;
      left: 6px;
      font-size: 32px;
    }
  }
}
</style>
