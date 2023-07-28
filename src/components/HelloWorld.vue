<template>
  <div class="hello">
    <div class="selectable" @mouseup="handleSelection">
      <h4>SELECT ANY PART OF THE TEXT AND SEE WHAT HAPPENS!</h4>
    </div>
    <div class="actions mt-3">
      <button @click="colorSelection">Color Selected Text</button>
      <button @click="deleteSelection">Delete Selected Text</button>
    </div>
  </div>
</template>

<script>
import Rangy from 'rangy';
Rangy.init();

export default {
  name: 'Hello',
  data() {
    return {
      selectionRange: null,
      selectedText: '',
    };
  },
  methods: {
    handleSelection() {
      const selection = Rangy.getSelection();
      const selectedText = selection.toString();
      const range = selection.getRangeAt(0);

      // Save the selected range for later use
      this.selectionRange = range;
      this.selectedText = selectedText;
      console.log('range', selectedText, range)
    },
    colorSelection() {
      const selection = Rangy.getSelection();
      console.log('selection', selection, selection.getRangeAt(0));
      if (selection.rangeCount > 0) {
        const range = selection.getRangeAt(0);
        range.applyCssClass('highlighted');
      }
    },

    deleteSelection() {
      const selectionRange = this.selectionRange;
      if (selectionRange) {
        selectionRange.deleteContents();
      }
    },
  },
  
};
</script>

<style scoped lang="scss">
.selectable {
  border: 1px solid #ccc;
  padding: 10px;
  cursor: text;
}

.highlighted {
  background-color: #ccc;
}

.actions {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

/* Add more styles as needed */

</style>
