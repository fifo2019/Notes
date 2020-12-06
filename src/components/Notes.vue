<template>
  <div class="notes">
    <div
      class="note"
      :class="{
        full: !grid,
        standart: note.priority === 'standart',
        important: note.priority === 'important',
        critical: note.priority === 'critical',
      }"
      v-for="(note, index) in notes"
      :key="note.id"
    >
      <div class="note-header">
        <p
          @click="changeTitleNote(note.id)"
          v-show="!note.change.title"
        >
          {{ note.title }}
        </p>
        <input
          type="text"
          v-show="note.change.title"
          v-model="note.change.title"
          @keyup.enter="saveTitleNote(note.id)"
          @keyup.esc="exitChangeTitleNote(note.id)"
        >
        <p style="cursor: pointer; margin: 0 0 0 10px;" @click="removeNote(index)">x</p>
      </div>
      <div class="note-body">
        <p
          @click="changeDescrNote(note.id)"
          v-show="!note.change.descr"
        >
          {{ note.descr }}
        </p>
        <textarea
          v-model="note.change.descr"
          v-show="note.change.descr"
          @keyup.enter="saveDescrNote(note.id)"
          @keyup.esc="exitChangeDescrNote(note.id)"
        ></textarea>
        <span>{{ note.date }}</span>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "notes",
    props: {
      notes: {
        type: Array, // Number, Array, Object, Boolean
        required: true,
        default: []
      },
      grid: {
        type: Boolean, // Number, Array, Object, Boolean
        required: true,
        default: true
      }
    },
    methods: {
      removeNote(index) {
        console.log(`Note id - ${index} removed`)
        this.$emit('remove', index)
      },
      changeTitleNote(id) {
        this.$emit('changeTitle', id)
      },
      changeDescrNote(id) {
        this.$emit('changeDescr', id)
      },
      saveTitleNote(id) {
        this.$emit('saveTitle', id)
      },
      exitChangeTitleNote(id) {
        this.$emit('exitChangeTitle', id)
      },
      saveDescrNote(id) {
        this.$emit('saveDescr', id)
      },
      exitChangeDescrNote(id) {
        this.$emit('exitChangeDescr', id)
      },
    }
  }
</script>

<style lang="scss" scoped>
  .notes {
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    padding: 40px 0;
  }

  .note {
    width: 46%;
    padding: 18px 20px;
    margin: 20px;
    background-color: #ffffff;
    transition: all .25s cubic-bezier(.02, .01, .47, 1);
    cursor: pointer;
    
    &:hover {
      box-shadow: 0 30px 30px rgba(0, 0, 0, .04);
      transform: translate(0, -6px);
      transition-delay: 0s !important;
    }
    
    &.full {
      width: 100%;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      text-align: center;
      p {
        margin-right: 16px;
        &.last-child {
          margin-right: 0;
        }
      }
    }
    
    &.standart {
      box-shadow: 0 30px 30px rgba(0, 0, 0, .04);
    }
  
    &.important {
      box-shadow: 0 0 30px rgb(255, 235, 59 / 61%);
    }
  
    &.critical {
      box-shadow: 0 0 30px #F44336;
    }
  }

  .note-header {
    display: flex;
    justify-content: space-between;

    p {
      font-size: 22px;
      color: #402caf;
    }
  }

  .note-body {
    p {
      margin: 20px 0;
    }

    span {
      font-size: 14px;
      color: #999999;
    }
  }
</style>