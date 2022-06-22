<template>
  <div class="container">
    <div class="mb-5">
      format：
      <Date-picker v-model:value="formatTime" valueType="format"></Date-picker>
      <time class="d-block mb-4">{{ formatTime }}</time>
      datetime：
      <Date-picker v-model:value="datetime" type="datetime"></Date-picker>
      <time class="d-block mb-4">{{ datetime }}</time>
      range：
      <Date-picker v-model:value="rangeTime" range></Date-picker>
      <time class="d-block mb-4">{{ rangeTime }}</time>
      timestamp：
      <Date-picker v-model:value="timestamp" timestamp></Date-picker>
      <time class="d-block mb-4">{{ timestamp }}</time>
      <hr>
      <section class="border-bottom mb-2 pb-3">
        <p class="fs-5 mb-0">限於今天到一週後的今天</p>
        <date-picker
          v-model:value="value1"
          :default-value="new Date()"
          :disabled-date="disabledBeforeTodayAndAfterAWeek"
        ></date-picker>
        <time class="d-block mb-4">{{ value1 }}</time>
      </section>
      <section class="border-bottom mb-2 pb-3">
        <p class="fs-5 mb-0">僅能選擇當前以後</p>
        <date-picker
          v-model:value="value3"
          value-type="format"
          type="time"
          placeholder="HH:mm"
          format="hh:mm a"
          confirm="true"
          :default-value="new Date().setHours(getDisabledTime)"
          :disabled-time="notBeforeNineOClock"
        ></date-picker>
        <time class="d-block mb-4">{{ value3 }}</time>
      </section>
      <section class="border-bottom mb-2 pb-3">
        <p class="fs-5 mb-0">今天 12:00 以後</p>
        <date-picker
          v-model:value="value4"
          type="datetime"
          confirm="true"
          :default-value="new Date().setHours(12, 0, 0, 0)"
          :disabled-date="notBeforeToday"
          :disabled-time="notBeforeTodayTwelveOClock"
          value-type="format"
        ></date-picker>
        <time class="d-block mb-4">{{ value4 }}</time>
      </section>
      <div>
        <p class="fs-5 mb-0">選擇特定時段</p>
        <date-picker
          v-model:value="fixedTimeList"
          :time-picker-options="{
            start: '08:30',
            step: '00:30',
            end: '18:30',
          }"
          format="hh:mm a"
          type="time"
          placeholder="hh:mm a"
        ></date-picker>
      </div>
    </div>
  </div>
</template>

<script>
import DatePicker from 'vue-datepicker-next'
import 'vue-datepicker-next/index.css'
import 'vue-datepicker-next/locale/zh-cn'
export default {
  components: {
    DatePicker
  },

  data () {
    return {
      formatTime: null,
      datetime: null,
      rangeTime: null,
      timestamp: null,
      value1: new Date(),
      value3: '',
      value4: '',
      fixedTimeList: null
    }
  },

  methods: {
    getDisabledTime () {
      return `${new Date().getHours()}, ${new Date().getMinutes()}`
    },
    disabledBeforeTodayAndAfterAWeek (date) {
      const today = new Date()
      today.setHours(0, 0, 0, 0)

      return date < today || date > new Date(today.getTime() + 7 * 24 * 3600 * 1000)
    },
    notBeforeNineOClock (date) {
      console.log(this.getDisabledTime())
      const getHou = new Date().getHours()
      const getMin = new Date().getMinutes()
      return date < new Date(date.getTime()).setHours(getHou, getMin)
    },
    notBeforeToday (date) {
      return date < new Date(new Date().setHours(0, 0, 0, 0))
    },
    notBeforeTodayTwelveOClock (date) {
      return date < new Date(new Date().setHours(12, 0, 0, 0))
    }
  },

  mounted () {
  }

}
</script>

<style lang='scss' scope></style>
