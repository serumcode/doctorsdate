<template>
  <div class="demo-app">
    <div class="demo-app-main">
      <FullCalendar
        class="demo-app-calendar"
        :options="calendarOptions"
        ref="cal"
      >
        <template v-slot:eventContent="arg">
          <b>{{ arg.timeText }}</b>
          <i>{{ arg.event.title }}</i>
        </template>
      </FullCalendar>
    </div>
    <v-dialog v-model="dialog" width="500">
      <v-card>
        <v-card-title class="text-h5 grey lighten-2"> Új esemény </v-card-title>

        <v-card-text>
          <v-menu
            ref="menu"
            v-model="menu"
            :close-on-content-click="false"
            :return-value.sync="currDate"
            transition="scale-transition"
            offset-y
            min-width="290px"
          >
            <template v-slot:activator="{ on }">
              <v-text-field
                rounded
                v-model="currDate"
                label="Dátum"
                prepend-inner-icon="fas fa-calendar-alt"
                readonly
                filled
                hide-details
                v-on="on"
                class="mt-5"
              ></v-text-field>
            </template>
            <v-date-picker v-model="currDate" no-title scrollable>
              <v-spacer></v-spacer>
              <v-btn text color="primary" @click="menu = false">Cancel</v-btn>
              <v-btn text color="primary" @click="$refs.menu.save(currDate)"
                >OK</v-btn
              >
            </v-date-picker>
          </v-menu>
          <v-menu
            ref="menu2"
            v-model="menuTime"
            :close-on-content-click="false"
            :return-value.sync="currTime"
            transition="scale-transition"
            offset-y
            min-width="290px"
          >
            <template v-slot:activator="{ on }">
              <v-text-field
                rounded
                v-model="currTime"
                label="Idő"
                prepend-inner-icon="fas fa-clock"
                readonly
                filled
                hide-details
                v-on="on"
                class="mt-5"
              ></v-text-field>
            </template>
            <v-time-picker
              style="transform: translateX(-50%); left: 50%"
              v-model="currTime"
              :allowed-minutes="allowedMinutes"
              :allowed-hours="allowedHours"
              class="mt-4"
              format="24hr"
            >
              <v-spacer></v-spacer>
              <v-btn text color="primary" @click="menu2 = false">Cancel</v-btn>
              <v-btn text color="primary" @click="$refs.menu2.save(currTime)"
                >OK</v-btn
              >
            </v-time-picker>
          </v-menu>
          <v-text-field
            rounded
            v-model="currTitle"
            label="Név"
            filled
            hide-details
            class="mt-5"
          ></v-text-field>
          <v-select
            class="mt-5"
            v-model="currType"
            :items="types"
            rounded
            filled
            return-object
            item-text="text"
            item-value="val"
            label="Ismétlés"
          ></v-select>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" @click="addEvent()"> Ok </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
import FullCalendar from "@fullcalendar/vue";
import dayGridPlugin from "@fullcalendar/daygrid";
import timeGridPlugin from "@fullcalendar/timegrid";
import interactionPlugin from "@fullcalendar/interaction";
import rrulePlugin from "@fullcalendar/rrule";
//import { INITIAL_EVENTS, createEventId } from './event-utils'
export default {
  components: {
    FullCalendar, // make the <FullCalendar> tag available
  },
  data: function() {
    return {
      dialog: false,
      currDate: null,
      currTime: null,
      currTitle: "",
      currType: { text: "Nincs ismétlődés", val: false },
      types: [
        { text: "Nincs ismétlődés", val: false },
        { text: "Minden héten", val: 1 },
        { text: "Páros héten", val: 2, start: 2 },
        { text: "Páratlan héten", val: 3, start: 1 },
      ],
      menu: false,
      menuTime: false,
      allowedMinutes: (v) => v === 0 || v === 30,
      allowedHours: [8, 9, 10, 11, 12, 13, 14, 15],
      calendarOptions: {
        plugins: [
          dayGridPlugin,
          timeGridPlugin,
          interactionPlugin,
          rrulePlugin,
        ],
        headerToolbar: {
          left: "prev,next week",
          center: "title",
          right: "dayGridMonth,timeGridWeek,timeGridDay",
        },
        navLinkDayClick: function(date) {
          console.log("day", date.toISOString());

          this.changeView("timeGridDay", date);
          // this.view.calendar.gotoDate( '2021-07-18')
        },
        dayClick: function(date, jsEvent, view) {
          alert("Clicked on: " + date.format(), jsEvent, view);
        },
        navLinks: true,
        initialView: "timeGridWeek",
        slotMaxTime: "16:00:00",
        slotMinTime: "08:00:00",
        events: [
          {
            id: 1,
            title: "keddess",
            start: "2021-06-08T08:00:00",
            end: "2021-06-08T10:00:00",
          },
          {
            id: 2,
            title: "páros hetes",
            start: "2021-01-04T10:00:00",
            end: "2021-01-04T12:00:00",
            dow: [2],
            rrule: {
              freq: "weekly",
              interval: 2,
              byweekday: ["mo"],
              dtstart: "2021-01-04T10:00:00",
              until: "2021-12-31",
            },
          },
          {
            id: 3,
            title: "páratlan hetes",
            start: "2021-01-01T12:00:00",
            end: "2021-01-01T16:00:00",
            rrule: {
              freq: "weekly",
              interval: 2,
              byweekday: ["we"],
              dtstart: "2021-01-01T12:00:00",
              until: "2021-12-31",
            },
          },
          {
            id: 4,
            title: "péntekes",
            start: "2021-01-01T10:00:00",
            end: "2021-01-01T16:00:00",
            rrule: {
              freq: "weekly",
              interval: 1,
              byweekday: ["fr"],
              dtstart: "2021-01-01T10:00:00",
              until: "2021-12-31",
            },
          },
          {
            id: 5,
            title: "csütörtökös",
            start: "2021-06-01T16:00:00",
            end: "2021-06-01T20:00:00",
            rrule: {
              freq: "weekly",
              interval: 1,
              byweekday: ["th"],
              dtstart: "2021-06-01T16:00:00",
              until: "2021-11-30",
            },
          },
        ],

        editable: true,
        selectable: true,
        selectMirror: true,
        dayMaxEvents: true,
        weekends: false,
        select: this.handleDateSelect,
        eventClick: this.handleEventClick,
        eventsSet: this.handleEvents,
        /* you can update a remote database when these fire:
        eventAdd:
        eventChange:
        eventRemove:
        */
      },
      currentEvents: [
        {
          title: "egy",
          start: "2021-07-23T10:30:00",
          end: "2021-07-23T10:50:00",
        },
        {
          title: "egy",
          start: "2021-07-22T10:30:00",
          end: "2021-07-23T10:50:00",
        },
      ],

      nameDays: ["mo", "tu", "we", "th", "fr"],
      calendarApi: null,
    };
  },
  methods: {
    formatTime(i) {
      if (i < 10) {
        i = "0" + i;
      }
      return i;
    },

    get_rrules(date, time) {
      //let o={};
      console.log("currtime", time);
      let date2 = new Date(date),
        start = date2.toISOString().slice(0, 10);
      var oneJan = new Date(date2.getFullYear(), 0, 1);

      // calculating number of days in given year before a given date
      var numberOfDays = Math.floor((date2 - oneJan) / (24 * 60 * 60 * 1000));

      // adding 1 since to current date and returns value starting from 0
      var result = Math.ceil((date2.getDay() + 1 + numberOfDays) / 7);
      console.log("res", result, time);
      if (this.currType.start === 2) {
        if (result % 2 !== 0) {
          date2.setDate(date2.getDate() + 1 * 7);
        }
        start = date2.toISOString().slice(0, 10);
      }
      let o = {
        freq: "weekly",
        interval: this.currType.val,
        byweekday: [this.nameDays[date2.getDay() - 1]],
        dtstart: start,
        until: "2021-12-31",
      };
      console.log("o", o);
      return o;
    },
    addEvent() {
      //todo addable end time
      let d = new Date(this.currDate + " " + this.currTime);

      let d_end = new Date(d.getTime() + 30 * 60000);
      let obj = {
        id: this.calendarOptions.events.length + 1,
        title: this.currTitle,
        start: this.currDate + "T" + this.currTime + ":00",
        end:
          d_end.toISOString().slice(0, 10) +
          "T" +
          this.formatTime(d_end.getHours()) +
          ":" +
          this.formatTime(d_end.getMinutes()) +
          ":00",
        //end:,this.formatTime(selectInfo.start.getHours()) + ":" + this.formatTime(selectInfo.start.getMinutes())
      };
      if (this.currType.val !== false) {
        obj["rrule"] = this.get_rrules(this.currDate, this.currTime);
      }
      console.log("obj", obj);
      this.calendarOptions.events.push(obj);
      console.log("calapi", this.calendarApi);
      // this.calendarApi.addEvent(obj);
      this.dialog = false;
    },
    handleDateSelect(selectInfo) {
      window.selectInfo = selectInfo;
      console.log("dialog", this.dialog, selectInfo);
      this.dialog = true;
      this.currDate = selectInfo.start.toISOString().slice(0, 10);
      this.currTime =
        this.formatTime(selectInfo.start.getHours()) +
        ":" +
        this.formatTime(selectInfo.start.getMinutes());
      this.calendarApi = selectInfo.view.calendar;
    },
    handleEventClick(clickInfo) {
      if (
        confirm(
          `Are you sure you want to delete the event '${clickInfo.event.title}'`
        )
      ) {
        clickInfo.event.remove();
      }
    },
    handleEvents(events) {
      this.currentEvents = events;
    },
  },
};
</script>

<style lang="scss">
h2 {
  margin: 0;
  font-size: 16px;
}
ul {
  margin: 0;
  padding: 0 0 0 1.5em;
}
li {
  margin: 1.5em 0;
  padding: 0;
}
b {
  /* used for event dates/times */
  margin-right: 3px;
}
.demo-app {
  display: flex;
  min-height: 100%;
  font-family: Arial, Helvetica Neue, Helvetica, sans-serif;
  font-size: 14px;
  .demo-app-sidebar {
    width: 300px;
    line-height: 1.5;
    background: #f98821;
    border-right: 1px solid #d3e2e8;
  }
  .demo-app-sidebar-section {
    padding: 2em;
  }
  .demo-app-main {
    flex-grow: 1;
    padding: 3em;
  }
  .fc {
    .fc-timegrid {
      background: white;
    }

    max-width: 1100px;
    margin: 0 auto;
    .fc-timegrid-slot {
      height: 3em;
    }
    .fc-button .fc-icon {
      vertical-align: middle;
      font-size: 1.5em;
      display: flex;
    }
    .fc-v-event {
      display: block;
      border: 1px solid #3788d8;
      border: 1px solid var(--primary);
      background-color: var(--primary);
    }
  }
  .fc-week-button {
    display: none;
  }
  .fc-theme-standard td,
  .fc-theme-standard th {
    border: 1px solid #c2d6da;
  }
  .fc-theme-standard th {
    background: var(--secondary);
    padding: 15px 0;
    a {
      color: white;
    }
  }
  .fc-daygrid-day.fc-day-today,
  .fc-timegrid-col.fc-day-today {
    background-color: var(--secondary--light) !important;
  }
}
</style>
