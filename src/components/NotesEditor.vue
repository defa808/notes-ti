<template>
  <div class="note-editor">
    <textarea v-model="text" placeholder="Enter your note here ..." rows="5"/>
    <div class="optionTools">
      <swatches
        class="colorPicker"
        @input="inputedColor"
        v-model="colors"
        show-fallback
        colors="text-advanced"
        popover-to="left"
      ></swatches>
      <button class="add-button" @click="createNote" :disabled="text.trim().length == 0">Add</button>
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

  data: () => ({
    text: "",
    colors: ""
  }),

  props: {
    note: { type: Object, required: false },
    currentNote: { type: Object }
  },

  methods: {
    createNote() {
      if (this.colors == "" || !this.colorInputed) {
        let getRandom = () => Math.floor(Math.random() * 255);
        this.colors = `rgb(${getRandom()},${getRandom()},${getRandom()})`;
      }
      if (this.currentNote != undefined) {
        this.currentNote.text = this.text;
        this.currentNote.bgcolor = this.colors;
      } else {
        const note = {
          id: new Date().getTime(),
          text: this.text,
          bgcolor: this.colors
        };

        this.$emit("createNote", note);
      }
      this.text = "";
    },
    inputedColor() {
      if (this.colors === "") this.colorInputed = false;
      else this.colorInputed = true;
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

.add-button:disabled {
  background: #2f6627;
}

.add-button {
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

.add-button:hover {
  background-color: #5cbf2a;
}

.add-button:active {
  position: relative;
  top: 1px;
}

.add-button:focus {
  outline: 0;
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

