<template>
  <div>
    <div class="options">
      <div class="form-group">
        <label>Controls</label>
        <div class="input-select">
          <div class="select-item">
            <input
              name="speed"
              id="realtime"
              type="radio"
              value="realtime"
              v-model="speed"
              @change="onSpeedChange"
            />
            <label for="realtime"><i class="fa fa-pause"></i></label>
          </div>

          <div class="select-item">
            <input
              name="speed"
              id="idealized"
              type="radio"
              value="idealized"
              v-model="speed"
              @change="onSpeedChange"
            />
            <label for="idealized"><i class="fa fa-play"></i></label>
          </div>

          <div class="select-item">
            <input name="speed" id="mute" type="radio" value="mute" />
            <label for="mute" @click="togglePlayMusic">
              <i v-if="playMusic" class="fa fa-volume-off"></i>
              <i v-else class="fa fa-volume-up"></i>
            </label>
          </div>

          <div class="select-item">
            <input name="speed" id="tickets" type="radio" value="mute" />
            <label for="tickets" @click="toggleTicketContainer">
              <i v-if="showTicketContainer" class="fa fa-eye-slash"></i>
              <i v-else class="fa fa-eye"></i>
            </label>
          </div>
        </div>
      </div>
    </div>
    <div v-if="showTicketContainer" class="tickets">
      <h4>Get your ticket here!</h4>
      <label>Cosmic Name </label>
      <input
        type="text"
        @keypress.enter="fetchTickets()"
        v-model="cosmicName"
      />
      <button @click="fetchTickets()">Fetch!</button>
      <button v-if="tickets[0].data.length" @click="clearTickets()">
        Clear
      </button>
      <div v-for="(ticket, index) in tickets[0].data" :key="index">
        <div class="ticket">
          <div class="left">
            <div class="image">
              <p class="admit-one">
                <span
                  ><img
                    class="sponsor-img"
                    src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQVj-3ZgTNfEZdmpaa1Fng2Iw_FvNQUQA5nmk4oBw7y&s"
                    alt="planetInfo.displayName"
                /></span>
                <span
                  ><img
                    class="sponsor-img"
                    src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSpGANoMqSG8nLVzrMdhuDcmb0UBTMf7f-Uk8x6fG918w&s"
                    alt="planetInfo.displayName"
                /></span>
                <span
                  ><img
                    class="sponsor-img"
                    src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRM5HoiIbFA5BvhKxZQSlzGRfEz4IxvSa0cIOCFTYEbZw&s"
                    alt="planetInfo.displayName"
                /></span>
              </p>
              <img
                class="planet-img"
                :src="`./assets/cards/${ticket.planet_image}.png`"
                alt="planetInfo.displayName"
              />
            </div>
            <div class="ticket-info">
              <p class="date">
                <span>{{ ticket.dayOfWeek }}</span>
                <span class="june-29">
                  {{ ticket.month }} {{ ticket.formattedDayOfMonth }}</span
                >
                <span> {{ ticket.year }}</span>
              </p>
              <div class="show-name">
                <h2>
                  {{
                    ticket.first_name.charAt(0).toUpperCase() +
                    ticket.first_name.slice(1)
                  }}
                  {{
                    ticket.last_name.charAt(0).toUpperCase() +
                    ticket.last_name.slice(1)
                  }}
                </h2>
              </div>
              <div class="time">
                <p>
                  EARTH<span> TO </span>{{ ticket.planet_image.toUpperCase() }}
                </p>
                <p>Travelling <span>@</span> {{ ticket.package }} Package</p>
              </div>
              <p class="location">
                <span>NJIT</span>
                <span class="separator"><i class="far fa-smile"></i></span
                ><span>GirlsHackathon 2023</span>
              </p>
            </div>
          </div>
          <div class="right">
            <p class="admit-one">
              <span
                ><img
                  class="sponsor-img"
                  src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRMMqM-7Xc8732C_8vx6ze1HnNVo2Rw25dQPeo3kMNW0g&s"
                  alt="planetInfo.displayName"
              /></span>
              <span
                ><img
                  class="sponsor-img"
                  src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR36VDb-06OycfZuYqcWIZRReofQgoQprCRWF9V5c3BdQ&s"
                  alt="planetInfo.displayName"
              /></span>
              <span
                ><img
                  class="sponsor-img"
                  src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQy-b6LuMASfHQLzJE-eo9vr12Vh6vDkRSTZf3dup2A&s"
                  alt="planetInfo.displayName"
              /></span>
            </p>
            <div class="right-info-container">
              <div class="time">
                <p>EARTH <span></span></p>
                <p><span>TO</span></p>
                <p>{{ ticket.planet_image.toUpperCase() }}</p>
                <p><span>@</span>{{ ticket.package }}</p>
              </div>
              <div class="barcode">
                <img
                  src="https://external-preview.redd.it/cg8k976AV52mDvDb5jDVJABPrSZ3tpi1aXhPjgcDTbw.png?auto=webp&s=1c205ba303c1fa0370b813ea83b9e1bddb7215eb"
                  alt="QR code"
                />
              </div>
              <p class="ticket-number">
                {{ parseFloat(ticket.price).toFixed(2) }} ETH
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      speed: "day_sec",
      playMusic: true,
      showTicketContainer: false,
      cosmicName: "",
      tickets: [
        {
          data: [],
        },
      ],
    };
  },
  emits: ["speedChanged"],
  methods: {
    getDayOfMonthSuffix(day) {
      if (day >= 11 && day <= 13) {
        return "th";
      }
      switch (day % 10) {
        case 1:
          return "st";
        case 2:
          return "nd";
        case 3:
          return "rd";
        default:
          return "th";
      }
    },
    onSpeedChange(e) {
      const value = e.target.value;
      this.$emit("speedChanged", value);
    },
    togglePlayMusic() {
      this.playMusic = !this.playMusic;
      this.$emit("musicUpdate", this.playMusic);
    },
    toggleTicketContainer() {
        this.showTicketContainer = !this.showTicketContainer;

    },
    clearTickets() {
      (this.cosmicName = ""),
        (this.tickets = [
          {
            data: [],
          },
        ]);
    },
    fetchTickets() {
      //   this.showTicket = true;
      axios
        .get(
          `https://total-treat-400007.uk.r.appspot.com/appointment/bookings?cosmic_handle=${this.cosmicName}`
        )
        .then((response) => {
          response.data.data.map((item) => {
            (item.month = this.convertAppointmentDate(
              item.appointment_date
            ).month),
              (item.dayOfWeek = this.convertAppointmentDate(
                item.appointment_date
              ).dayOfWeek),
              (item.formattedDayOfMonth = this.convertAppointmentDate(
                item.appointment_date
              ).formattedDayOfMonth),
              (item.year = this.convertAppointmentDate(
                item.appointment_date
              ).year);
          });
          this.tickets[0].data = response.data.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    convertAppointmentDate(appointmentDateStr) {
      // const appointmentDateStr = "2023-10-04T00:00:00.000Z";
      const appointmentDate = new Date(appointmentDateStr);

      // Get the day of the week
      const daysOfWeek = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      const dayOfWeek = daysOfWeek[appointmentDate.getDay()];

      const months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      const month = months[appointmentDate.getMonth()];

      // Get the day of the month with the appropriate suffix (e.g., "1st", "2nd", "3rd", "4th", etc.)
      const dayOfMonth = appointmentDate.getDate();
      const dayOfMonthSuffix = this.getDayOfMonthSuffix(dayOfMonth);
      const formattedDayOfMonth = `${dayOfMonth}${dayOfMonthSuffix}`;

      // Get the year
      const year = appointmentDate.getFullYear();
      console.log(`${dayOfWeek}, ${formattedDayOfMonth}, ${year}`);
      return {
        dayOfWeek: dayOfWeek,
        formattedDayOfMonth: formattedDayOfMonth,
        year: year,
        month: month,
      };
    },
  },
  mounted() {
    this.$emit("speedChanged", this.speed);
  },
};
</script>

<style scoped lang="scss">
label {
  font-family: "Orbitron", sans-serif;
}

@import url("https://fonts.googleapis.com/css2?family=Staatliches&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Nanum+Pen+Script&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body,
html {
  height: 100vh;
  display: grid;
  font-family: "Staatliches", cursive;
  background: #d83565;
  color: black;
  font-size: 14px;
  letter-spacing: 0.1em;
}

.ticket {
  margin: 10px;
  display: flex;
  padding: 5px;
  background: white;
  border-radius: 10px;
  box-shadow: rgba(0, 0, 0, 0.3) 0px 19px 38px,
    rgba(0, 0, 0, 0.22) 0px 15px 12px;
}

.left {
  display: flex;
}

.planet-img {
  height: 250px;
}

.image {
  height: 250px;
  width: 250px;
  // background-image: url("https://media.pitchfork.com/photos/60db53e71dfc7ddc9f5086f9/1:1/w_1656,h_1656,c_limit/Olivia-Rodrigo-Sour-Prom.jpg");
  background-size: contain;
  opacity: 0.85;
}

.sponsor-img {
  height: 40px;
  width: 40px;
}

.admit-one {
  position: absolute;
  color: darkgray;
  height: 250px;
  padding: 0 10px;
  letter-spacing: 0.15em;
  display: flex;
  text-align: center;
  justify-content: space-around;
  writing-mode: vertical-rl;
  //   transform: rotate(-180deg);
}

.admit-one span:nth-child(2) {
  color: white;
  font-weight: 700;
}

.left .ticket-number {
  height: 250px;
  width: 250px;
  display: flex;
  justify-content: flex-end;
  align-items: flex-end;
  padding: 5px;
}

.ticket-info {
  padding: 10px 30px;
  display: flex;
  flex-direction: column;
  text-align: center;
  justify-content: space-between;
  align-items: center;
}

.date {
  border-top: 1px solid gray;
  border-bottom: 1px solid gray;
  padding: 5px 0;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: space-around;
  color: #d83565;
}

.date span {
  width: 100px;
}

.date span:first-child {
  text-align: left;
}

.date span:last-child {
  text-align: right;
}

.date .june-29 {
  color: #d83565;
  font-size: 20px;
}

.show-name {
  font-size: 32px;
  font-family: "Nanum Pen Script", cursive;
  color: #d83565;
}

.show-name h1 {
  font-size: 48px;
  font-weight: 700;
  letter-spacing: 0.1em;
  color: #4a437e;
}

.time {
  padding: 10px 0;
  color: #4a437e;
  text-align: center;
  display: flex;
  flex-direction: column;
  gap: 10px;
  font-weight: 700;
}

.time span {
  font-weight: 400;
  color: gray;
}

.left .time {
  font-size: 16px;
}

.location {
  color: #d83565;
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 100%;
  padding-top: 8px;
  border-top: 1px solid gray;
}

.location .separator {
  font-size: 20px;
}

.right {
  width: 180px;
  border-left: 1px dashed #404040;
}

.right .admit-one {
  color: darkgray;
}

.right .admit-one span:nth-child(2) {
  color: gray;
}

.right .right-info-container {
  height: 250px;
  padding: 10px 10px 10px 35px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
}

.right .show-name h1 {
  font-size: 18px;
}

.barcode {
  height: 100px;
}

.barcode img {
  height: 100%;
}

.right .ticket-number {
  color: gray;
}

.tickets {
  position: absolute;
  right: 10px;
  top: 320px;
  background-color: var(--primary);
  border-radius: var(--radius);
  padding: 10px 10px 10px 10px;
  max-height: 600px;
  overflow-y: auto;
}

.options {
  position: absolute;
  right: 10px;
  top: 100px;

  .form-group {
    margin: 15px 5px;
    text-align: center;

    > label {
      font-size: 18px;
    }
  }

  .input-select {
    background-color: var(--primary);
    border-radius: var(--radius);
    margin-top: 10px;
    max-width: 115px;
    margin-left: auto;
    overflow: hidden;

    .select-item {
      display: flex;
      justify-content: stretch;
      align-items: stretch;
      text-align: center;

      input {
        appearance: none;

        &:checked + label {
          background-color: var(--tertiary);
        }
      }

      label {
        padding: 10px 16px;
        cursor: pointer;
        flex-grow: 1;
      }
    }
  }

  .input-check {
    appearance: none;
    margin-top: 8px;
    width: 1em;
    height: 1em;
    background-color: var(--text);
    outline: 2px solid #fff;
    border-radius: 50%;
    cursor: pointer;
    margin-right: 10px;

    &:checked {
      background-color: var(--tertiary);
    }

    & + label {
      cursor: pointer;
    }
  }
}

@media (max-height: 360px) {
  .options {
    top: 50px;
  }
}
</style>
