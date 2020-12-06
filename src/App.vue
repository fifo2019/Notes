<template>
  <div>
    <div class="wrapper">
    <!-- <header></header> -->

      <div class="wrapper-content">
        <section class="container">

          <!-- warring message  -->
          <message v-if="message" :message="message" />

          <!-- form add note  -->
          <newNote :note="note" @addNote="addNote" />

          <!--  title   -->
          <div class="note-header" style="margin: 36px 0;">
            <h1>{{ title }}</h1>
  
            <!-- search -->
            <search
              :value="search"
              placeholder="Find your note"
              @search="search = $event"
            />
            
            <!-- icons controls -->
            <div class="icons">
              <svg :class="{ active: grid }" @click="grid = true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" ><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>
              <svg :class="{ active: !grid }" @click="grid = false" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><line x1="3" y1="6" x2="3" y2="6"></line><line x1="3" y1="12" x2="3" y2="12"></line><line x1="3" y1="18" x2="3" y2="18"></line></svg>
            </div>
          </div>

          <!-- conclusion note on page  -->
          <notes
            :notes="notesFilter"
            :grid="grid"
            @remove="removeNote"
            @changeTitle="changeTitleNote"
            @changeDescr="changeDescrNote"
            @saveTitle="saveTitleNote"
            @exitChangeTitle="exitChangeTitleNote"
            @saveDescr="saveDescrNote"
            @exitChangeDescr="exitChangeDescrNote"
          />

        </section>
      </div>

    <!-- <footer></footer>-->
    </div>
  </div>
</template>

<script>
  import message from '@/components/Message.vue'
  import newNote from '@/components/NewNote.vue'
  import notes from '@/components/Notes.vue'
  import search from '@/components/Search.vue'

  export default {
    components: {
      message,
      newNote,
      notes,
      search
    },
    data() {
      return {
        title: 'Notes App',
        search: '',
        message: null,
        grid: true,
        note: {
          id: '',
          title: '',
          descr: '',
          priority: 'standart',
          change: {
            title: false,
            descr: false,
          }
        },
        notes: [
          {
            id: 'First',
            title: 'First Note',
            descr: 'Description for first note',
            date: new Date(Date.now()).toLocaleString(),
            priority: 'standart',
            change: {
              title: '',
              descr: '',
            }
          },
          {
            id: 'Second',
            title: 'Second Note',
            descr: 'Description for second note',
            date: new Date(Date.now()).toLocaleString(),
            priority: 'important',
            change: {
              title: '',
              descr: '',
            }
          },
          {
            id: 'Third',
            title: 'Third Note',
            descr: 'Description for third note',
            date: new Date(Date.now()).toLocaleString(),
            priority: 'critical',
            change: {
              title: '',
              descr: '',
            }
          }
        ]
      }
    },
    computed: {
      notesFilter() {
        let array = this.notes,
            search = this.search
        
        if (!search) return array
        
        // Small
        search = search.trim().toLowerCase()
        
        // Filter
        array = array.filter(function (item) {
          if (item.title.toLowerCase().indexOf(search) !== -1) {
            return item
          }
        })
        
        // Error
        return array
      }
    },
    methods: {
      addNote() {
        let {title, descr, priority, change} = this.note

        if (title === '') {
          this.message = 'title can`t blank'
          return false
        }

        this.notes.push({
          id: `id-${new Date().valueOf()}`,
          title,
          descr,
          date: new Date(Date.now()).toLocaleString(),
          priority,
          change
        })

        this.message = null
        this.note.title = ''
        this.note.descr = ''
        this.note.priority = 'standart'
      },
      removeNote(index) {
        this.notes.splice(index, 1)
      },
      changeTitleNote(id) {
        this.notes.filter(item => {
          if (item.id === id) {
            item.change.title = item.title
          }
        })
      },
      changeDescrNote(id) {
        this.notes.filter(item => {
          if (item.id === id) {
            item.change.descr = item.descr
          }
        })
      },
      saveTitleNote(id) {
        this.notes.filter(item => {
          if (item.id === id) {
            item.title = item.change.title
            item.date = new Date(Date.now()).toLocaleString()
            item.change.title = ''
          }
        })
      },
      exitChangeTitleNote(id) {
        this.notes.filter(item => {
          if (item.id === id) {
            item.change.title = ''
          }
        })
      },
      saveDescrNote(id) {
        this.notes.filter(item => {
          if (item.id === id) {
            item.descr = item.change.descr
            item.date = new Date(Date.now()).toLocaleString()
            item.change.descr = ''
          }
        })
      },
      exitChangeDescrNote(id) {
        this.notes.filter(item => {
          if (item.id === id) {
            item.change.descr = ''
          }
        })
      },
    }
  }
</script>

<style lang="scss" scoped>
  .note-header {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;

    h1 {
      font-size: 32px;
    }

    p {
      font-size: 22px;
      color: #402caf;
    }

    svg {
      margin-right: 12px;
      color: #999999;
      cursor: pointer;
      &.active {
        color: #402caf;
      }
      &.last-child {
        margin-right: 0;
      }
    }
  }
</style>
