<template>
  <div>
    <div class="container">
      <div class="header">
        <h3>{{ months[currentMonth] }} {{ currentYear }}</h3>
      </div>
      <div class="calendar">
        <div v-for="weekday in weekdays" :key="weekday" class="weekdays">
          <strong>{{ weekday }}</strong>
        </div>
        <div
          v-for="(day, index) in monthdays"
          :key="index"
          :class="['monthdays', (day === currentDay && 'active'), (day.currentMonth && 'current-month')]"
        >
          <div class="day">
            <p>{{ day.day }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "VueCalendar",
  data() {
    return {
      currentDay: null,
      currentMonth: null,
      currentYear: null,
      months: [
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
      ],
      weekdays: [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ],
      monthdays: [],
      numShowMonths: 3,
    };
  },
  methods: {
    getCurrentDay() {
      const today = new Date();
      this.currentDay = today.getDate();
      this.currentMonth = today.getMonth();
      this.currentYear = today.getFullYear();
    },

    getMonthDays() {
      let lastDayOfMonth = new Date(this.currentYear, this.currentMonth + 1, 0).getDate();
      let firstWeekDay = new Date(this.currentYear, this.currentMonth, 1).getDay();
      let lastDayOfPrevMonth = new Date(this.currentYear, this.currentMonth, 0).getDate();
      
      for(let i = (lastDayOfPrevMonth - firstWeekDay + 1); i <= lastDayOfMonth; i++){
        this.monthdays.push({currentMonth: false, day: i});
      }

      for (let i = 1; i <= lastDayOfMonth; i++) {
        this.monthdays.push({currentMonth: true, day: i});
      }

      let nextMonth = 7 - (this.monthdays.length % 7)
      for(let i = 1; i<= nextMonth; i++){
        this.monthdays.push({currentMonth: false, day: i});
      }


    },
  },
  mounted() {
    this.getCurrentDay();
    this.getMonthDays();
  },
};
</script>

<style>
.active {
  background-color: red;
}
</style>