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
        </div>
      </div>
    </div>
    <div class="tickets">Get your ticket here!</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      speed: "day_sec",
      playMusic: true,
    };
  },
  emits: ["speedChanged"],
  methods: {
    onSpeedChange(e) {
      const value = e.target.value;
      this.$emit("speedChanged", value);
    },
    togglePlayMusic() {
      this.playMusic = !this.playMusic;
      this.$emit("musicUpdate", this.playMusic);
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

.tickets {
  position: absolute;
  right: 10px;
  top: 400px;
  background-color: var(--primary);
  border-radius: var(--radius);
  padding: 10px 200px 10px 200px;
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
