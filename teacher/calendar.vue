<template>
<div>
  <inline-calendar
  ref="calendar"
  @on-change="onChange"
  @on-view-change="onViewChange"
  class="inline-calendar-demo"
  :show.sync="show"
  v-model="value"
  start-date="2019-06-01"
  :end-date="endDate"
  :range="range"
  :show-last-month="showLastMonth"
  :show-next-month="showNextMonth"
  :highlight-weekend="highlightWeekend"
  :return-six-rows="return6Rows"
  :hide-header="hideHeader"
  :hide-week-list="hideWeekList"
  :replace-text-list="replaceTextList"
  :weeks-list="weeksList"
  :render-function="buildSlotFn"
  :disable-past="disablePast"
  :disable-future="disableFuture"
  :disable-weekend="disableWeekend"
  :disable-date-function="disableDateFunction">
  </inline-calendar>

  <group>
    <cell :title="'日期'" :value="value"></cell>
    <form-preview v-for="(item,index) in list" :header-label="item.studentname" :header-value="item.subjectname" :body-items="info[index]" :key="index"></form-preview>
  </group>

  <div style="margin: 15px;">
    <x-button type="primary" @click.native="$refs.calendar.switchViewToToday()">switchViewToToday</x-button>
  </div>

</div>
</template>

<script>
import { InlineCalendar, Group, XSwitch, Radio, XButton, Cell, Divider,FormPreview } from 'vux'
import dayjs from 'dayjs'
import moment from 'moment'
export default {
  data () {
    return {
      endDate: dayjs().add(30, 'd').format('YYYY-MM-DD'),
      show: true,
      value: '',
      listValue: '',
      range: false,
      showLastMonth: true,
      showNextMonth: true,
      highlightWeekend: false,
      return6Rows: true,
      hideHeader: false,
      hideWeekList: false,
      replaceTextList: {},
      replace: false,
      changeWeeksList: false,
      weeksList: [],
      useCustomFn: false,
      buildSlotFn: () => '',
      disablePast: false,
      disableFuture: false,
      disableWeekend: false,
      disableDateFunction (date) {
        if (date.formatedDate === '2017-10-16') {
          return true
        }
      },
      list:[],
      info:[],
      events:[],
    }
  },
  created() {
    this.getList()
  },
  methods: {
    onChange (val) {
      console.log(val)
      this.list = []
      this.info = []
      for(var i = 0 ; i < this.events.length ; i++){
         if(this.events[i].date == val){
           let temp = [{
             label:'时间：',
             value:this.events[i].start
           }]
           this.info.push(temp)
           this.list.push(this.events[i])
         }
      }
      console.log(this.list)
    },
    onViewChange (val, count) {
      console.log('on view change', val, count)
    },
    getList() {
      console.log(this.$store.getters)
      this.$http.get('http://139.199.168.158:8080/admin/teacher_allevents?id='+this.$store.getters.userid).then((response) => {
         response = response.data
         console.log(response)
         if (response.code === 1) {
           console.log('获取排课记录成功')
           for(var i = 0 ; i < response.data.length ; i++){
           let temp={
             id:this.events.length+1,
             date:moment(response.data[i].COURSE_TIME).format('YYYY-MM-DD'),
             start:moment(response.data[i].COURSE_TIME).format('HH:mm:ss'),
             title:response.data[i].SUBJECT_NAME+'\n'+'学生:'+response.data[i].STUDENT_NAME  +'\n'+'教室:'+response.data[i].ROOM_ID,
             timerankid:response.data[i].TIMERANK_ID,
             teachername:response.data[i].TEACHER_NAME,
             studentid:response.data[i].STUDENT_ID,
             studentname:response.data[i].STUDENT_NAME,
             eventid:response.data[i].id,
             subjectname:response.data[i].SUBJECT_NAME+' 课程',
             coursetime:response.data[i].COURSE_TIME,
             room:response.data[i].ROOM_ID,
           }
           this.events.push(temp)   
          }
          console.log(this.events)
         }
         else{
           console.log('排课记录失败')
         }
    })
    this.$http.get('http://139.199.168.158:8080/admin/teacher_homework?id='+this.$store.getters.userid).then((response) => {
         response = response.data
         console.log(response)
         if (response.code === 1) {
           console.log('获取作业成功')
          //  this.eventlist=response.data
           console.log(response)
           for(var i = 0 ; i < response.data.length ; i++){
           console.log(response.data[i])
           let temp={
             id:this.events.length+1,
             date:moment(response.data[i].SUBMISSION_DATE).format('YYYY-MM-DD'),
             start:moment(response.data[i].SUBMISSION_DATE).format('HH:mm:ss'),
             subjectname:response.data[i].SUBJECT_NAME+' 作业',
             studentname:response.data[i].STUDENT_NAME,
             title:response.data[i].HOMEWORK_ID,
             allDay:true,
             eventid:response.data[i].id
           }
           this.events.push(temp)   
          }
          console.log(this.events)
         }
         else{
           console.log('获取作业失败')
         }
    })
    // constants.log(this.$store.getters.targetUser)
    }, 
  },
  
  watch: {
    replace (val) {
      this.replaceTextList = val ? {
        'TODAY': '今'
      } : {}
    },
    useCustomFn (val) {
      this.buildSlotFn = val ? (line, index, data) => {
        return /8/.test(data.date) ? '<div style="font-size:12px;text-align:center;"><span style="display:inline-block;width:5px;height:5px;background-color:red;border-radius:50%;"></span></div>' : '<div style="height:19px;"></div>'
      } : () => ''
    },
    changeWeeksList (val) {
      this.weeksList = val ? ['日', '一', '二', '三', '四', '五', '六 '] : ['Su', 'Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa']
    }
  },
  components: {
    InlineCalendar,
    Group,
    XSwitch,
    Radio,
    XButton,
    Cell,
    Divider,
    FormPreview
  }
}
</script>

<style lang="less" scoped>
.inline-calendar-demo {
  background: rgba(255,255,255,0.9);
}
</style>
