<template>
  <main class="m-5">
    <form action="#" class="border text-center p-5" @submit.prevent="getDate">
      <div class="mb-2">
        <label for="StartTime">起始日期：</label>
        <input type="date" id="StartTime" :style="dateNullStyle" ref="StartTime" v-model="courseDate.StartTime" @change="getDate">
        <i class="text-danger ms-2" v-if="courseDate.StartTime===''">請選擇起始日期</i>
      </div>
      <div class="mb-2">
        <label for="EndTime">結束日期：</label>
        <input type="date" id="EndTime" :style="dateNullStyle" ref="EndTime" v-model="courseDate.EndTime" @change="getDate">
        <i class="text-danger ms-2" v-if="courseDate.EndTime===''">請選擇結束日期</i>
      </div>
    </form>
    <template v-if="this.courseDate.StartTime && this.courseDate.EndTime">
      選擇的課程日期：{{ courseDate.StartTime }} ~ {{ courseDate.EndTime }}
      <br>
      開始：{{ new Date(courseDate.StartTime).getTime() }}
      <br>
      結束：{{ new Date(courseDate.EndTime).getTime() }}
    </template>
  </main>
<DatePicker></DatePicker>

</template>

<script>
import DatePicker from '@/components/DatePicker.vue'
export default {
  computed: {
    dateNullStyle () {
      return {
        border: this.dateNull ? '1px solid red' : '',
        color: this.dateNull ? 'red' : ''
      }
    },
    dateNull () {
      if (this.courseDate.StartTime === '' && this.courseDate.EndTime === '') {
        return true
      } else {
        return false
      }
    }
  },
  components: {
    DatePicker
  },
  data () {
    return {
      datetime: '',
      courseDate: {
        StartTime: '',
        EndTime: ''
      }
    }
  },
  methods: {

  },
  mounted () {

  }
}
</script>
<style lang="scss" scoped>

</style>
