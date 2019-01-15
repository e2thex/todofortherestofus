<template>
  <section class="">
    <div>
      <!--logo/-->
      <h1 class="title bg-orange-dark text-center w-full p-2">
        To Do For The Rest Of.Us
      </h1>
      <div class="flex items-center flex-wrap m-auto container">
        <div class="w-full">
          <c-l-list
            :items="items"
            :add="addItem"
            class="w-full"/>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Logo from '~/components/Logo.vue'
import CLList from '~/components/CLList.vue'
import CLNewTask from '~/components/CLNewTask.vue'
export default {
  components: {
    Logo,
    CLList,
    CLNewTask
  },
  data() {
    return {
      items: []
    }
  },
  mounted() {
    if (this.localStorage().db) {
      let data = JSON.parse(this.localStorage().db)
      data.forEach(item => this.initItem(item.name, item.date, item.repeat))
      this.sort()
    }
  },
  methods: {
    localStorage() {
      console.log(process.client)
      return process.client ? localStorage : {}
    },
    db() {
      if (!this.localStorage().db) {
        this.localStorage().db = JSON.stringify([])
      }
      let db = {}
      db.data = this.items
      db.save = () => {
        this.localStorage().db = JSON.stringify(this.items)
        return db
      }
      db.load = () => {
        if (process.client) {
          db.data = JSON.parse(this.localStorage().db)
        } else {
          db.data = []
        }
        return db
      }
      db.push = a => {
        db.data.push(a)
        return db
      }
      db.load()
      return db
    },
    initItem(name, date, repeat, isNew) {
      let item = { name, date, repeat }
      if (isNew) {
        item.isNew = true
      }
      item.check = () => {
        let oneDay = 24 * 60 * 60 * 1000
        if (item.repeat > 0) {
          item.date = new Date(Date.now() + oneDay * item.repeat)
        } else {
          item.deleteOnSort = true
        }
        this.sort()
      }
      item.delete = () => {
        item.deleteOnSort = true
        this.sort()
      }
      item.save = update => {
        let nItem = { ...item, ...update }
        item.name = nItem.name
        item.repeat = nItem.repeat
        item.date = nItem.date
        delete item.isNew
        this.sort()
      }
      this.items.push(item)
    },
    addItem() {
      this.initItem('', Date(), 0, true)
      console.log(this.items)
      console.log('nowsort')
      this.sort()
    },
    saveItems() {
      if (process.client) {
        console.log(this.items)
        console.log(JSON.stringify(this.items))
        localStorage.db = JSON.stringify(this.items)
      }
    },
    sort() {
      console.log(this.items)
      this.items = this.items.filter(item => !item.deleteOnSort)
      this.items.sort((a, b) => {
        if (a.isNew) {
          return -100000000000
        }
        return Date.parse(a.date) - Date.parse(b.date)
      })
      console.log(this.items)
      console.log('callsave')
      this.saveItems()
    }
  }
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/

.container {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  margin: auto;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  color: #35495e;
}

.subtitle {
  font-weight: 300;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
