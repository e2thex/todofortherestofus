<template>
  <span>
    <svg
      class = "h-full w-full"
      fill-rule="evenodd"
      image-rendering="optimizeQuality"
      shape-rendering="geometricPrecision"
      text-rendering="geometricPrecision"
      viewBox="0 0 22.5729 22.1329"
      xmlns="http://www.w3.org/2000/svg"
    >
      <path
        :fill="color"
        d="m1.8597,22.115h18.8535c1.01302,0 1.84175-.81385 1.84175-1.80869v-15.1689c0-.99484-.82873-1.80869-1.84175-1.80869h-1.26627v2.11916c0,.44474-.36388.80862-.80862.80862h-3.07934c-.44474,0-.80862-.36388-.80862-.80862v-2.11916h-7.0021v2.11939c0,.44474-.36388.80862-.80862.80862h-3.07934c-.44474,0-.80862-.36388-.80862-.80862v-2.11939h-1.19195c-1.01302,0-1.84175.81385-1.84175,1.80869v15.1689c0,.99484.82873,1.80869 1.84175,1.80869z"
        stroke="#1f1a17"
        stroke-width=".0942"
      />
      <path
        d="m2.8789 20.8h16.81c0.93107 0 1.6928-0.57388 1.6928-1.2754v-11.64h-20.195v11.64c0 0.70152 0.76171 1.2754 1.6928 1.2754z"
        fill="#fff"
        stroke="#1f1a17"
        stroke-width=".0942"
      />
      <path
        d="m5.1273 0.0471h0.52814a1.203 1.203 0 0 1 1.203 1.203v3.1689a1.203 1.203 0 0 1 -1.203 1.203h-0.52814a1.203 1.203 0 0 1 -1.203 -1.203v-3.1689a1.203 1.203 0 0 1 1.203 -1.203"
        fill="#1f1a17"
        stroke="#1f1a17"
        stroke-width=".0942"
      />
      <path
        d="m16.825 0.04714h0.52814a1.203 1.203 0 0 1 1.203 1.203v3.1689a1.203 1.203 0 0 1 -1.203 1.203h-0.52814a1.203 1.203 0 0 1 -1.203 -1.203v-3.1689a1.203 1.203 0 0 1 1.203 -1.203"
        fill="#1f1a17"
        stroke="#1f1a17"
        stroke-width=".0942"
      />
      <text 
        :class="{'hidden': !isThisMonth || isThisWeek}"
        x="50%" 
        y="18" 
        style="font-size:12;text-align:center"
        text-anchor="middle">{{ day }}</text>
      <text 
        :class="{'hidden': !isThisWeek || isToday}"
        x="50%" 
        y="18" 
        style="font-size:9;text-align:center" 
        text-anchor="middle">{{ dayOfWeek }}</text>
      <text 
        :class="{'hidden': isThisMonth}"
        x="50%" 
        y="13.5" 
        style="font-size:6;text-align:center" 
        text-anchor="middle">{{ month }}</text>
      <text 
        :class="{'hidden': isThisMonth}"
        x="50%" 
        y="19" 
        style="font-size:6;text-align:center" 
        text-anchor="middle" >{{ day }}</text>
      <text 
        :class="{'hidden': !isToday}"
        x="50%" 
        y="16" 
        style="font-size:6.5;text-align:center" 
        text-anchor="middle">Today</text>
    </svg>
  </span>
</template>

<script>
export default {
  props: {
    date: {
      type: String,
      default: Date(Date.now())
    }
  },
  computed: {
    dateObj() {
      return new Date(Date.parse(this.date))
    },
    day() {
      console.log(this.dateObj.getDate())
      let day = this.dateObj.getDate()
      return day < 10 ? '0' + day : day
    },
    dayOfWeek() {
      let days = ['Sun', 'Mon', 'Tue', 'Wed', 'Thr', 'Fri', 'Sat']
      return days[this.dateObj.getDay()]
    },
    month() {
      let m = [
        'Jan',
        'Feb',
        'Mar',
        'Apr',
        'May',
        'Jun',
        'Jul',
        'Aug',
        'Sep',
        'Oct',
        'Nov',
        'Dec'
      ]
      return m[this.dateObj.getMonth()]
    },
    text() {
      let now = new Date(Date.now())
      let oneDay = 24 * 60 * 60 * 1000
      let date = new Date(Date.parse(this.date))
      console.log(date)
      if (this.sameDay(now, date)) {
        return ''
      } else if (this.sameWeek(now, date)) {
        let days = ['Sun', 'Mon', 'Tue', 'Wed', 'Thr', 'Fri', 'Sat']
        return days[date.getDay()]
      } else {
        let d = new Date(date)
        return d.getDate()
      }
    },
    color() {
      let now = new Date(Date.now())
      now.setHours(0, 0, 0, 0)
      let date = new Date(Date.parse(this.date))
      return now >= date ? 'red' : 'green'
    },
    isThisMonth() {
      let now = new Date(Date.now())
      let date = new Date(Date.parse(this.date))
      return this.sameMonth(now, date)
    },
    isThisWeek() {
      let now = new Date(Date.now())
      let date = new Date(Date.parse(this.date))
      return this.sameWeek(now, date)
    },
    isToday() {
      let now = new Date(Date.now())
      let date = new Date(Date.parse(this.date))
      return this.sameDay(now, date)
    }
  },
  methods: {
    sameDay(d1, d2) {
      return (
        d1.getDate() === d2.getDate() &&
        d1.getMonth() === d2.getMonth() &&
        d1.getFullYear() === d2.getFullYear()
      )
    },
    sameWeek(d1, d2) {
      function getWeek(date) {
        let onejan = new Date(date.getFullYear(), 0, 1)
        let start = new Date(date.getFullYear(), 0, 1 + 7 - onejan.getDay())
        let oneDay = 24 * 60 * 60 * 1000
        console.log(start)
        console.log(start.getDay())
        return Math.floor((date - start) / oneDay / 7)
      }
      console.log(getWeek(d1))
      console.log(getWeek(d2))
      return (
        getWeek(d1) === getWeek(d2) && d1.getFullYear() === d2.getFullYear()
      )
    },
    sameMonth(d1, d2) {
      return (
        d1.getMonth() === d2.getMonth() && d1.getFullYear() === d2.getFullYear()
      )
    }
  }
}
</script>

<style scoped>
</style>
