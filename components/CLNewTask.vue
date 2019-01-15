<template>
  <div>
    <h2>New Task</h2>
    <div class="w-1/4 flex flex-wrap">
      <input 
        v-model="name" 
        name="task"
        class="h-2 border w-3/4">
      <div
        class="w-1/4 pl-1"
      >
        <datepicker 
          ref="startDatePicker"
          v-model="start"
          class=""
          input-class="hidden"
        />
        <div 
          @click="chooseDate"
        >
          <c-l-date
            :date="start"
            class="w-full cursor-pointer"
          />
        </div>
      </div>
      <select 
        v-model="repeat"
        name="repeat" 
        class="h-2 border w-3/4">
        <option value = "0">Single Event</option>
        <option value = "1">Every Day</option>
        <option value = "7">Once a Week</option>
        <option value = "12">Once every 2 Weeks</option>
      </select>
      <input 
        type="submit" 
        value="Add" 
        class="border rounded w-1/4 hover:bg-grey bg-grey-lighter pl-1"
        @click="submit">
    </div>
  </div>
</template>

<script>
import Datepicker from 'vuejs-datepicker'
import CLDate from '~/components/CLdate.vue'
export default {
  components: {
    Datepicker,
    CLDate
  },
  props: {
    add: {
      type: Function,
      default: () => {}
    }
  },
  data: function() {
    return {
      start: Date(Date.now()),
      name: '',
      repeat: '7'
    }
  },
  methods: {
    submit() {
      this.add(this.name, this.start, this.repeat)
      this.name = ''
      this.repeat = '7'
    },
    chooseDate() {
      console.log('hi')
      console.log(this.$refs)
      this.$refs.startDatePicker.showCalendar()
    }
  }
}
</script>
<style scoped>
</style>
