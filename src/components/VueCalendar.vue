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
          :class="[
            'monthdays',
            day.day === currentDay && 'today',
            day.currentMonth && 'current-month',
          ]"
        >
          <div class="day">
            <p>{{ day.day }}</p>
          </div>
          <div class="events">
            <div
              v-for="event in eventsForDay(day.day)"
              :key="event.id"
              :class="eventStyle(day.day, event)"
            >
              <div class="event-title">{{ eventTitle(day.day, event) }}</div>
            </div>
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
      events: [
        {
          id: 1,
          start: "2024-01-09",
          end: "2024-01-14",
          title: "Caroline Caleguer",
        },
        {
          id: 2,
          start: "2024-01-18",
          end: "2024-01-22",
          title: "Anderson Stefano",
        },
      ],
    };
  },
  methods: {
    getCurrentDay() {
      const today = new Date();
      this.currentDay = today.getDate();
      console.log(this.currentDay);
      this.currentMonth = today.getMonth();
      this.currentYear = today.getFullYear();
    },

    getMonthDays() {
      let lastDayOfMonth = new Date(
        this.currentYear,
        this.currentMonth + 1,
        0
      ).getDate();
      let firstWeekDay = new Date(
        this.currentYear,
        this.currentMonth,
        1
      ).getDay();
      let lastDayOfPrevMonth = new Date(
        this.currentYear,
        this.currentMonth,
        0
      ).getDate();

      for (
        let i = lastDayOfPrevMonth - firstWeekDay + 1;
        i <= lastDayOfMonth;
        i++
      ) {
        this.monthdays.push({ currentMonth: false, day: i });
      }

      for (let i = 1; i <= lastDayOfMonth; i++) {
        this.monthdays.push({ currentMonth: true, day: i });
      }

      let nextMonth = 7 - (this.monthdays.length % 7);
      for (let i = 1; i <= nextMonth; i++) {
        this.monthdays.push({ currentMonth: false, day: i });
      }
    },

    eventsForDay(day) {
      const dateStr = `2024-${(this.currentMonth + 1)
        .toString()
        .padStart(2, "0")}-${day.toString().padStart(2, "0")}`;
      return this.events.filter(
        (event) => event.start <= dateStr && event.end >= dateStr
      );
    },

    eventStyle(day, event) {
      const dateStr = `2024-${(this.currentMonth + 1)
        .toString()
        .padStart(2, "0")}-${day.toString().padStart(2, "0")}`;
      return event.start === dateStr
        ? "event-start"
        : event.end === dateStr
        ? "event-end"
        : "event";
    },

    eventTitle(day, event){
        const dateStr = `2024-${(this.currentMonth + 1)
        .toString()
        .padStart(2, "0")}-${day.toString().padStart(2, "0")}`;
        return event.start === dateStr ? event.title : ""
    }

  },
  mounted() {
    this.getCurrentDay();
    this.getMonthDays();
  },
};
</script>

<style>
</style>