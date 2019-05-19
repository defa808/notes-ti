<template>
  <div class="note-editor">
    <textarea v-model="currentNoteData.text" placeholder="Enter your note here ..." rows="5"/>
    <div class="optionTools">
      <swatches
        class="colorPicker"
        @input="inputedColor"
        v-model="currentNoteData.bgcolor"
        show-fallback
        colors="text-advanced"
        popover-to="left"
      ></swatches>
      <button
        class="button"
        @click="createNote"
        :disabled="currentNote.text.trim().length == 0"
      >Add</button>
      <button
        class="button cancel-button"
        @click="cancelNote"
        v-if="$parent.findNoteById(currentNoteData.id)" 
        :disabled="currentNote.text.trim().length == 0"
      >Cancel</button>
    </div>
    <!-- <photoshop-picker v-model="colors"/> -->
  </div>
</template>

<script>
import "vue-swatches/dist/vue-swatches.min.css";
import Swatches from "vue-swatches";

export default {
  name: "notes-editor",

  components: {
    Swatches
  },

  data: function() {
    return {
      currentNoteData: this.currentNote
    };
  },

  props: {
    currentNote: { type: Object }
  },

  methods: {
    createNote() {
      if (this.currentNoteData.text == "") return;
      if (this.currentNoteData.id == undefined) {
        this.currentNoteData.id = new Date().getTime();
        if (this.currentNoteData.bgcolor == "" || !this.colorInputed) {
          let getRandom = () => Math.floor(Math.random() * 255);
          this.currentNoteData.bgcolor = `rgb(${getRandom()},${getRandom()},${getRandom()})`;
        }
      }
      this.currentNoteData.tags = this.getTags(this.currentNoteData.text);
      this.$emit("createNote", this.currentNoteData);
      this.currentNoteData = { id: undefined, text: "" };
      if (!this.colorInputed) this.currentNoteData.bgcolor = "";
    },

    cancelNote() {
      this.currentNoteData = { id: undefined, text: "" };
    },

    inputedColor() {
      if (this.colors === "") this.colorInputed = false;
      else this.colorInputed = true;
    },

    getTags(textNote) {
      let tags = textNote.match(/(?:|^)#[A-Za-z0-9\-\.\_]+\b/g);
      return tags != null ? tags : [];
    }
  }
};
</script>

<style lang="scss">
.note-editor {
  width: 100%;
  max-width: 600px;
  padding: 16px;
  margin: 16px auto;
  background-color: white;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
  border-radius: 2px;
  display: flex;
  flex-direction: column;
}

textarea {
  width: 100%;
  resize: none;
  margin: 5px;
  font-size: 14px;
  border: none;
  font-weight: 300;
}

textarea:focus {
  outline: 0;
}

.button:disabled {
  background: #2f6627;
}

.button {
  align-self: flex-end;
  width: 100px;
  margin-left: 50px;
  background-color: #44c767;
  border-radius: 8px;
  border: 1px solid #18ab29;
  cursor: pointer;
  color: #ffffff;
  font-size: 14px;
  padding: 8px 8px;
  text-transform: uppercase;
  text-decoration: none;
  text-shadow: 0px 1px 0px #2f6627;
}

.button:hover {
  background-color: #5cbf2a;
}

.button:active {
  position: relative;
  top: 1px;
}

.button:focus {
  outline: 0;
}

.cancel-button{
  margin-left: 20px;
}

.colorPicker {
  width: 20px;
}

.optionTools {
  width: 100%;
  display: flex;
  justify-content: flex-end;
}
</style>

