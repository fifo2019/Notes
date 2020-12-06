<template>
  <div class="new-note">
    <label>Title
      <input v-model="note.title" type="text" style="margin: 10px;">
    </label>
    
    <label>Description
      <textarea v-model="note.descr" style="margin: 10px;"></textarea>
    </label>
  
    <h2>Priority</h2>
    <div class="priority-wrp">
      <label>Standart
        <input
          type="radio"
          v-model="note.priority"
          value="standart"
          checked
        >
      </label>
      <label>Important
        <input
          type="radio"
          v-model="note.priority"
          value="important"
        >
      </label>
      <label>Critical
        <input
          type="radio"
          v-model="note.priority"
          value="critical"
        >
      </label>
    </div>
    
    <button class="btn btnPrimary" @click="addNote">New note</button>
  </div>
</template>

<script>
  export default {
    name: "newNote",
    props: {
      note: {
        type: Object, // Number, Array, Object, Boolean
        required: true,
        default: {
          title: '',
          descr: '',
          priority: 'standart'
        }
      }
    },
    methods: {
      addNote() {
        this.$emit('addNote', this.note)
      }
    }
  }
</script>

<style lang="scss" scoped>
  .new-note {
    text-align: center;
  }
  
  .priority-wrp {
    display: flex;
    justify-content: space-around;
    margin: 20px;
    
    & label {
      cursor: pointer;
    }
    
    
  }
</style>