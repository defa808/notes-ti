<template>
  <div class="note" :style="{backgroundColor: note.bgcolor}">
    <link
      rel="stylesheet"
      href="https://use.fontawesome.com/releases/v5.2.0/css/all.css"
      integrity="sha384-hWVjflwFxL6sNzntih27bfxkr27PmbbK/iSvJ+a4+0owXq79v+lsFkW54bOGbiDQ"
      crossorigin="anonymous"
    >
    <span class="edit-note" @click="$parent.$parent.editNote(note.id)">
      <i class="far fa-edit"></i>
    </span>
    <span class="delete-note" @click="$parent.$parent.deleteNote(note.id)">x</span>
    <span>{{ note.text }}</span>
  </div>
</template>

<script>
export default {
  name: "node-item",
  props: {
    note: {
      type: Object,
      required: true
    }
  }
};
</script>

<style>
.note {
  width: 200px;
  height: auto;
  float: left;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
  border-radius: 2px;
  padding: 10px;
  margin-bottom: 10px;
  transition: box-shadow 0.3s;
  word-wrap: break-word;
  position: relative;
}

.note:hover {
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.19), 0 6px 6px rgba(0, 0, 0, 0.23);
}

.edit-note {
  position: absolute;
  top: 5px;
  right: 15px;
  display: none;
  color: rgba(0, 0, 0, 0.6);
  cursor: pointer;
}

.delete-note {
  position: absolute;
  top: 5px;
  right: 5px;
  display: none;
  color: rgba(0, 0, 0, 0.6);
  cursor: pointer;
}

.note:hover .delete-note {
  display: block;
}

.note:hover .edit-note {
  display: block;
}

</style>
