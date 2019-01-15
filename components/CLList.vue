<template>
  <div class ="flex relative flex-wrap">
    <div 
      class="lg:w-1/3 items-center flex justify-center md:w-1/2 w-full xl:w-1/4 cursor-pointer hover:bg-grey-lighter rounded"
      @click="add"
    >
      <c-l-add />
    </div>
    <template 
      v-for="item in items" >
      <c-l-list-item 
        :item="item" 
        :key="item.name" 
        class ="lg:w-1/3 xl:w-1/4 md:w-1/2 w-full"
      />
    </template>
  </div>
</template>
<script>
import CLListItem from '~/components/CLListItem.vue'
import CLAdd from '~/components/CLAdd.vue'
export default {
  components: {
    CLListItem,
    CLAdd
  },
  props: {
    items: {
      type: Array,
      default: () => []
    },
    add: {
      type: Function,
      default: () => {}
    }
  },
  created() {
    console.log('inlist')
    console.log(this.items)
  },
  methods: {
    addNew() {
      console.log('add')
    }
  }
}
</script>

<style scoped>
</style>
