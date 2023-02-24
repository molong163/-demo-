<template>
  <div>
    <div class="fc-toolbar">
      <div class="fc-left">
        <el-button-group>
          <el-button @click="month">
            月
          </el-button>
          <el-button @click="week">
            周
          </el-button>
          <el-button @click="today">
            今天
          </el-button>
        </el-button-group>
      </div>
      <div class="fc-center">
        <el-button @click="prev">
          上一个
        </el-button>
        <h2>
          {{ title }}
        </h2>
        <el-button @click="next">
          下一个
        </el-button>
      </div>
      <el-button
        ref="next"
        class="search"
        type="button"
        @click="search"
      >
        查询
      </el-button>
    </div>

    <FullCalendar
      id="calendar"
      ref="fullCalendar"
      class="demo-app-calendar"
      :options="calendarOptions"
    >
      <template v-slot:eventContent="arg">
        <el-popover
          placement="top"
          title="标题"
          width="200"
          :visible-arrow="false"
          trigger="hover"
        >
          <i class="title">{{ arg.event.title }}</i>
          <el-button @click="more">
            更多
          </el-button>
          <div
            slot="reference"
            class="popper-content"
          >
            <span>{{ arg.timeText }}</span>
            <i>{{ arg.event.title }}</i>
            <i>{{ arg.event.title }}</i>
          </div>
        </el-popover>
      </template>
    </FullCalendar>
  </div>
</template>
<script>
import FullCalendar from '@fullcalendar/vue'
import dayGridPlugin from '@fullcalendar/daygrid'
import timeGridPlugin from '@fullcalendar/timegrid'
import listPlugin from '@fullcalendar/list'
import interactionPlugin from '@fullcalendar/interaction'

export default {
  components: {
    FullCalendar // make the <FullCalendar> tag available
  },
  data() {
    return {
      title: '',
      calendarOptions: {
        locale: 'zh',
        plugins: [dayGridPlugin, timeGridPlugin, listPlugin, interactionPlugin],
        initialView: 'dayGridMonth',
        editable: true,
        dayMaxEvents: true, // allow "more" link when too many events
        eventDurationEditable: true, // 可以调整事件的时间
        selectable: true, // 日历格子可选择
        nowIndicator: true, // 当前的时间线显示
        eventDisplay: 'block', // 争对全天的情况下，以块状显示
        headerToolbar: false, // 隐藏头部的导航栏
        selectMirror: false,
        displayEventEnd: true, // like 08:00 - 13:00
        eventTimeFormat: { // like '14:30:00'
          hour: '2-digit',
          minute: '2-digit',
          meridiem: false,
          hour12: false // 设置时间为24小时
        },
        slotLabelFormat: { // 左侧时间格式
          hour: '2-digit',
          minute: '2-digit',
          meridiem: 'lowercase',
          hour12: false // false设置时间为24小时
        },
        events: [
          {
            id: 1,
            title: 'event 1',
            start: '2022-10-21',
            color: 'purple'
          },
          {
            id: 2, title: 'event 2', start: '2022-10-22', color: 'purple'
          },
          { title: 'event 3', start: '2022-10-23' },
          { title: 'event 4', start: '2022-10-24' },
          { title: 'event 5', start: '2022-10-21' },
          { title: 'event 6', start: '2022-10-21', color: 'purple' },
          { title: 'event 7', start: '2022-10-21' },
          {
            id: 3,
            title: 'event 6',
            start: '2022-11-21',
            end: '2022-11-23',
            color: 'purple',
            extendedProps: {
              description: 'asdasdasdasdasdasdasdasds'
            },
            allDay: false
          },
          {
            id: 4,
            title: 'event 7',
            start: '2022-11-21',
            extendedProps: {
              description: '444444444444'
            }
          }
        ],
        eventColor: '#378006', // 事件的颜色
        allDayText: '全天',
        droppable: true // this allows things to be dropped onto the calendar
      },
      calendarApi: null,
      monthEvent: [
        {
          id: 'number_1',
          resourceId: 'number_1',
          title: 'event 1',
          start: '2022-10-21',
          color: 'purple'
        },
        {
          resourceId: 'number_2',
          id: 'number_2', title: 'event 2', start: '2022-10-22', color: 'purple'
        },
        { title: 'event 3', start: '2022-10-23' },
        { title: 'event 4', start: '2022-10-24' },
        { title: 'event 5', start: '2022-10-21' },
        { title: 'event 6', start: '2022-10-21', color: 'purple' },
        { title: 'event 7', start: '2022-10-21' },
        {
          id: 'number_3',
          resourceId: 'number_3',
          title: 'event9996',
          start: '2022-10-21',
          end: '2022-10-26',
          color: 'purple',
          extendedProps: {
            description: 'asdasdasdasdasdasdasdasds'
          }
        },
        {
          id: 4,
          title: 'event 7',
          start: '2022-11-22',
          extendedProps: {
            description: '444444444444'
          }
        }
      ],
      weekEvent: [
        {
          id: 'number_1',
          resourceId: 'number_1',
          title: 'week_event',
          start: '2022-11-11',
          color: 'purple'
        }
      ]
    }
  },
  watch: {
    // 切换视图显示不同的事件
    'calendarApi.view.type'() {
      this.getDtata()
    }
  },
  mounted() {
    this.calendarApi = this.$refs.fullCalendar.getApi()
    this.title = this.calendarApi.view?.title
    // 模拟动态获取数据
    this.getDtata()
  },
  methods: {
    getDtata() {
      setTimeout(() => {
        this.calendarOptions.events = this.calendarApi.view?.type === 'dayGridMonth' ? this.monthEvent : this.weekEvent
      }, 200)
    },
    prev() {
      this.calendarApi.prev()
      this.title = this.calendarApi.view?.title
    },
    next() {
      this.calendarApi.next()
      this.title = this.calendarApi.view?.title
    },
    today() {
      this.calendarApi.today()
      this.title = this.calendarApi.view?.title
    },
    month() {
      this.calendarApi.changeView('dayGridMonth')
      this.calendarApi.today()
      this.title = this.calendarApi.view?.title
    },
    week() {
      this.calendarApi.changeView('timeGridWeek')
      this.calendarApi.today()
      this.title = this.calendarApi.view?.title
    },
    day() {
      this.calendarApi.today()
      this.title = this.calendarApi.view?.title
    },
    search() {
      this.calendarApi.changeView('dayGrid', {
        start: '2022-10-21',
        end: '2022-10-23'
      })
    }
  }
}
</script>
