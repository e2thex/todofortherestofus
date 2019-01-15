<template>
  <div 
    class="flex p-3 flex-wrap hover:bg-grey-lighter hover:cursor-pointer rounded" 
  >
    <div 
      v-if="!edit" 
      :style = "{opacity: opacity}"
      class="flex flex-wrap"
      @click="done"
    >
      <div class="w-3/4 p-2 text-left">
        {{ item.name }}
        <span 
          class="align-bottom"
          @click.prevent.stop="startEditing"
        >
          <c-l-edit class="h-1"/>
        </span>
      </div>
      <c-l-date 
        :date="item.date"
        class="w-1/4 block"
      />
    </div>
    <template v-else> 
      <input 
        v-model="editItem.name" 
        name="task"
        class="h-2 border w-3/4">
      <div
        class="w-1/4 pl-1"
      >
        <datepicker 
          ref="startDatePicker"
          v-model="editItem.date"
          class=""
          input-class="hidden"
        />
        <div 
          @click="chooseDate"
        >
          <c-l-date
            :date="editItem.date"
            class="w-full cursor-pointer"
          />
        </div>
      </div>
      <select 
        v-model="editItem.repeat"
        name="repeat" 
        class="h-2 border w-3/4">
        <option value = "0">Single Event</option>
        <option value = "1">Every Day</option>
        <option value = "2">Every Other Day</option>
        <option value = "3">Every 3 Days</option>
        <option value = "7">Once a Week</option>
        <option value = "12">Once every 2 Weeks</option>
      </select>
      <input 
        type="submit" 
        value="Save" 
        class="border rounded w-1/4 hover:bg-grey bg-grey-lighter pl-1"
        @click="submit">
    </template>
  </div>
</template>

<script>
import CLDate from '~/components/CLDate.vue'
import CLEdit from '~/components/CLEdit.vue'
import Datepicker from 'vuejs-datepicker'
export default {
  components: {
    CLDate,
    Datepicker,
    CLEdit
  },
  props: {
    item: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      edit: this.item.isNew,
      editItem: { ...this.item }
    }
  },
  computed: {
    opacity() {
      let date = Date.parse(this.item.date)
      let now = Date.now()
      let oneDay = 24 * 60 * 60 * 1000
      return date > now ? Math.min(1, 0.1 + (1 * oneDay) / (date - now)) : 1
    }
  },
  created() {
    console.log('initem')
    console.log(this.item)
  },
  methods: {
    done() {
      console.log(this.item.date)
      this.item.check()
    },
    submit() {
      this.item.save(this.editItem)
      this.edit = false
    },
    chooseDate() {
      this.$refs.startDatePicker.showCalendar()
    },
    startEditing() {
      console.log(this.edit)
      console.log('Edit!')
      this.edit = true
      console.log(this.edit)
    }
  }
}
</script>

<style scoped>
</style>
