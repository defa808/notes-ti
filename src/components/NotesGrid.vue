<template>
  <div class="notes-grid" ref="grid">
    <note-item v-for="note in notes" :key="note.id" :note="note"/>
  </div>
</template>

<script>
import Masonry from "masonry-layout";
import NoteItem from "@/components/NoteItem";
export default {
  name: "notes-grid",
  props: {
    notes: {
      type: Array,
      required: true
    }
  },
  components: { NoteItem },

  mounted() {
    const grid = this.$refs.grid;
    this.msnry = new Masonry(grid, {
      itemSelector: ".note",
      columnWidth: 200,
      gutter: 10,
      isFitWidth: true
    });
  },
  updated() {
    this.msnry.reloadItems();
    this.msnry.layout();
  }
};
</script>

<style lang="scss">
.notes-grid {
  margin: 0 auto;
}
</style>
