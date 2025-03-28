<script>
import { defineComponent } from 'vue'
import FullCalendar from '@fullcalendar/vue3'
import dayGridPlugin from '@fullcalendar/daygrid'
import timeGridPlugin from '@fullcalendar/timegrid'
import interactionPlugin from '@fullcalendar/interaction'
import AddEventForm from './components/AddEventForm.vue'
import EditForm from './components/EditForm.vue'
import SideBar from './components/SideBar.vue'
let eventGuid = 3;
export function createEventId() {
  return String(eventGuid++)
}

export default defineComponent({
  components: {
    SideBar,
    FullCalendar,
    AddEventForm,
    EditForm
  },
  data() {
    return {
      isForm: false,
      isEditForm: false,
      formLeft: 300,
      parent: '.app-calendar',
      info: {},
      event: {
        title: '',
        date: '',
        time: '',
        color: '#3b86ff',
      },
      calendarOptions: {
        plugins: [
          dayGridPlugin,
          timeGridPlugin,
          interactionPlugin // needed for dateClick
        ],
        headerToolbar: {
          left: 'today,prev,next',
          center: 'title',
          right: 'dayGridMonth,timeGridWeek,timeGridDay,myCustomButton',
        },
        buttonText: {
          prev: 'back',
          next: 'next'
        },
        customButtons: {
          myCustomButton: {
            text: 'Agenda',
            click: function () {
              alert('Agenda clicked the custom button!');
            }
          }
        },
        initialView: 'dayGridMonth',
        editable: true,
        selectable: true,
        selectMirror: true,
        dayMaxEvents: true,
        weekends: true,
        select: this.handleDateSelect,
        eventClick: this.handleEventClick,
        eventsSet: this.handleEvents,
        events: [
          { id: 1, title: 'First Event', date: '2025-03-12', color: '#378006' },
        ],
        nowIndicator: true,
      },
      clickInfo: {},
    }
  },
  methods: {
    saveEvent(e) {
      this.event = e;
      this.calendarOptions.events = [
        ...this.calendarOptions.events,
        {
          id: createEventId(),
          title: this.event.title,
          start: this.event.date + 'T' + this.event.time,
          color: this.event.color
        }
      ];
      this.hideForm();
      this.hideEditForm();
    },
    saveEditedEvent(e) {
      this.event = e;
      this.calendarOptions.events = [
        ...this.calendarOptions.events,
        {
          id: this.event.id,
          title: this.event.title,
          start: this.event.date + 'T' + this.event.time,
          color: this.event.color
        }
      ];
      this.hideForm();
      this.hideEditForm();
    },
    handleWeekendsToggle() {
      this.calendarOptions.weekends = !this.calendarOptions.weekends // update a property
    },
    showForm() {
      this.isForm = true;
    },
    hideForm() {
      this.isForm = false;
    },
    handleDateSelect(selectInfo) {
      this.showForm();
      let calendarApi = selectInfo.view.calendar;
      calendarApi.unselect() // clear date selection
    },
    handleEventClick(clickInfo) {
      let eventData = this.calendarOptions.events.filter(item => item.id === clickInfo.event.id);
      this.showEditForm();
      this.info = eventData;
      this.clickInfo = clickInfo.event;
    },
    showEditForm() {
      this.isEditForm = true;
    },
    hideEditForm() {
      this.isEditForm = false;
    },
    deleteEvent() {
      this.clickInfo.remove();
      this.hideEditForm();
    }
  }
})

</script>

<template>
  <div class="app">
    <SideBar />
    <div class="app-main">
      <h1>Calendar</h1>
      <div class="calendar">
        <h2>Calendar View</h2>
        <FullCalendar class="app-calendar" :options="calendarOptions">
        </FullCalendar>
        <AddEventForm v-if="isForm" :isForm="isForm" @submitted="saveEvent" @formCancelled="hideForm" />
        <EditForm v-if="isEditForm" :isEditForm="isEditForm" :eventData="info" @submitted="saveEditedEvent"
          @formCancelled="hideEditForm" @deleteEvent="deleteEvent" />
      </div>
    </div>
  </div>
</template>

