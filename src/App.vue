<template>
  <div class="notes-app">
    <notes-header :title="title"/>
    <notes-editor
      @createNote="createNote"
      :currentNote="currentNote"
      :key="currentNote.id"
      @editNote="editNote"
    />
    <div v-if="filterByHashTag != ''" class="filterBlock" @click="clearFilter">{{filterByHashTag}}</div>
    <notes-grid
      :notes="noteComputed"
      @deleteNote="deleteNote"
      @setFilterByHashTag="setFilterByHashTag"
    />
  </div>
</template>

<script>
import Vue from "vue";
import { library } from "@fortawesome/fontawesome-svg-core";
import { faCoffee } from "@fortawesome/free-solid-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

library.add(faCoffee);

Vue.component("font-awesome-icon", FontAwesomeIcon);

Vue.config.productionTip = false;

import NotesHeader from "@/components/NotesHeader";
import NotesEditor from "@/components/NotesEditor";
import NotesGrid from "@/components/NotesGrid";
// import notes from "@/notes.json";

const notes = localStorage.notes ? JSON.parse(localStorage.notes) : [];

export default {
  name: "notes-app",
  components: {
    NotesHeader,
    NotesEditor,
    NotesGrid
  },

  data: () => ({
    title: "Notes App",
    text: "",
    bgcolor: "",
    id: new Date().getTime(),
    filterByHashTag: "",
    notes
  }),

  methods: {
    deleteNote(nodeId) {
      this.notes = this.notes.filter(note => note.id != nodeId);
      localStorage.notes = JSON.stringify(this.notes);
    },

    createNote(note) {
      let indexCurrent = this.notes.findIndex(x => x.id == note.id);
      if (indexCurrent != -1) this.notes.splice(indexCurrent, 1, note);
      else this.notes.push(note);

      localStorage.notes = JSON.stringify(this.notes);
      this.id = new Date().getTime();
      this.text = '';      
    },

    editNote(noteId) {debugger;
      let note = this.notes.find(x => x.id == noteId);
      this.id = note.id;
      this.text = note.text;
      this.bgcolor = note.bgcolor;
    },

    setFilterByHashTag(hashTag) {
      this.filterByHashTag = hashTag;
    },

    clearFilter() {
      this.filterByHashTag = "";
    },

    findNoteById(id){
      return this.notes.findIndex(x=> x.id == id) != -1;
    }

  },

  

  computed: {
    currentNote: function() {
      return {
        id: this.id,
        text: this.text,
        bgcolor: this.bgcolor
      };
    },

    noteComputed() {
      if (this.filterByHashTag != "")
        return this.notes.filter(x => x.tags.includes(this.filterByHashTag));
      return this.notes;
    }
  }
};
</script>

<style lang="scss">
$color: #eaeaea;

* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  font-weight: 300;
  background-color: $color;
}

.notes-app {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 980px;
  margin: 0 auto;
}

.filterBlock {
  cursor: pointer;
  padding: 10px;
  border-radius: 50px;
  margin-bottom: 20px;
  display: block;
  padding: 10px 20px;
   box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
  transition: all 0.3s cubic-bezier(.25,.8,.25,1);
}
</style>
