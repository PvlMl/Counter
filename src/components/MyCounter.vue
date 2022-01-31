<template>
  <div class="mt-10">
    <div v-if="!isFormOpen">
      <section
        v-if="!isEnouth"
        class="
          text-3xl
          flex
          justify-center
          content-center
          flex-col
          mx-auto
          text-center
        "
      >
        Spent
      </section>
      <section
        v-if="isEnouth"
        class="
          text-3xl
          flex
          justify-center
          content-center
          flex-col
          mx-auto
          text-center
        "
      >
        Too late
      </section>
      <section class="flex text-6xl justify-center content-center">
        <div class="mr-2 relative">
          {{ getFormattedTime(displayDays) }}
          <div class="text-sm bottom-0">days</div>
        </div>
        <span>:</span>
        <div class="mx-2 relative">
          {{ getFormattedTime(displayHours) }}
          <div class="text-sm bottom-0">hours</div>
        </div>
        <span>:</span>
        <div class="mx-2 relative">
          {{ getFormattedTime(displayMinutes) }}
          <div class="text-sm bottom-0">minuts</div>
        </div>
        <span>:</span>
        <div class="ml-2 relative">
          {{ getFormattedTime(displaySeconds) }}
          <div class="text-sm bottom-0">seconds</div>
        </div>
      </section>
    </div>
    <div v-if="isFormOpen">
      <div class="flex justify-center mt-10">
        <p>Days</p>
        : &nbsp;<input
          v-model="remainingDays"
          type="text"
          class="border-4 mr-10"
        />
        <p>Hours</p>
        : &nbsp;<input
          v-model="remainingHours"
          type="text"
          class="border-4 mr-10"
        />
        <p>Minutes</p>
        : &nbsp;<input
          v-model="remainingMin"
          type="text"
          class="border-4 mr-10"
        />
        <p>Seconds</p>
        : &nbsp;<input
          v-model="remainingSec"
          type="text"
          class="border-4 mr-10"
        />
      </div>
      <div class="flex justify-center mt-16">
        <button
          @click="startCounter"
          class="
            bg-red-900
            hover:bg-red-500
            text-white
            font-bold
            py-2
            px-4
            rounded
          "
        >
          RUN
        </button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      displayDays: 0,
      displayHours: 0,
      displayMinutes: 0,
      displaySeconds: 0,
      remainingSec: 0,
      remainingMin: 0,
      remainingHours: 0,
      remainingDays: 0,
      totalNumberOfSeconds: 0,
      timer: null,
      isEnouth: false,
      isFormOpen: true,
    };
  },
  methods: {
    getRemainingPeriod() {
      const actualDate = new Date();
      const finalDate = new Date(
        actualDate.getFullYear(),
        actualDate.getMonth(),
        actualDate.getDate() + +this.remainingDays,
        actualDate.getHours() + +this.remainingHours,
        actualDate.getMinutes() + +this.remainingMin,
        actualDate.getSeconds() + +this.remainingSec,
        0
      );
      this.totalNumberOfSeconds = Math.floor((finalDate - actualDate) / 1000);
    },
    getFormattedTime(n) {
      return n < 10 ? "0" + n : n;
    },
    startCounter() {
      if (!this.haveTime) return;
      this.isFormOpen = false;
      let rest;
      this.getRemainingPeriod();
      this.timer = setInterval(() => {
        this.displayDays = Math.floor(this.totalNumberOfSeconds / 3600 / 24);
        rest = this.totalNumberOfSeconds - this.displayDays * 3600 * 24;
        this.displayHours = Math.floor(rest / 3600);
        rest -= this.displayHours * 3600;
        this.displayMinutes = Math.floor(rest / 60);
        rest -= this.displayMinutes * 60;
        this.displaySeconds = rest;
        this.totalNumberOfSeconds -= 1;
        if (this.totalNumberOfSeconds < 0) {
          clearInterval(this.timer);
          this.isEnouth = true;
        }
      }, 1000);
    },
  },
  computed: {
    haveTime() {
      return (
        this.remainingDays ||
        this.remainingHours ||
        this.remainingMin ||
        this.remainingSec
      );
    },
  },
};
</script>

<style>
</style>