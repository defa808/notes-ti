<template>
  <div class="notes-app">
    <notes-header :title="title"/>
    <notes-editor @createNote="createNote" :note="currentNote" @editNote="editNote"/>
    <notes-grid :notes="notes" @deleteNote="deleteNote"/>
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
    notes,
    currentNote: undefined
  }),

  methods: {
    deleteNote(nodeId) {
      this.notes = this.notes.filter(note => note.id != nodeId);
      localStorage.notes = JSON.stringify(notes);
    },

    createNote(note) {
      this.notes.push(note);
      localStorage.notes = JSON.stringify(notes);
    },
    editNote(noteId) {
      debugger;
      let note = this.notes.find(x => x.id == noteId);
      this.currentNote = note;
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
</style>
