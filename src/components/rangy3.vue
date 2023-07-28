<template>
    <div>
        <div>
            <h3>Text Editor</h3>
            <div class="text-editor" ref="myTextContainer" @mouseup="handleSelection">
                <b-tooltip :show.sync="showTooltip" target="myTextContainer">
                    <div class="color-palette-tooltip actions" v-show="showTooltip">
                        <div class="color-option" v-for="color in colors" :key="color" :style="{ backgroundColor: color }"
                            @click="changeSelectionColor(color)"></div>
                    </div>
                </b-tooltip>
                <span v-for="(text, index) in editorContent" :key="index" :style="{ backgroundColor: text.color }">
                    {{ text.content }}
                </span>
            </div>
        </div>
    </div>
</template>
  
<script>
import rangy from 'rangy/lib/rangy-core';

export default {
    data() {
        return {
            colors: ['#ff0000', '#00ff00', '#0000ff'], // Sample color options
            selectedRanges: [],
            selectedColor: '#ff0000', // Initially set to red (#ff0000)
            showTooltip: false,
            // editorContent: [],
            editorContent: [
                { content: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. ', color: 'red' },
                { content: 'Sed tincidunt tellus nec ex hendrerit, quis scelerisque odio semper.', color: 'red' },
            ],
        };
    },
    methods: {
        initializeEditorContent() {
            // Initialize the editor content array with each character of the original text and default color
            this.editorContent = this.editorText.split('').map((char) => ({ content: char, color: '' }));
        },
        handleSelection() {
            const selection = rangy.getSelection();
            if (!selection.isCollapsed) {
                const selectedText = selection.toString();
                const startIndex = this.editorText.indexOf(selectedText);
                const endIndex = startIndex + selectedText.length;
                for (let i = startIndex; i < endIndex; i++) {
                    this.editorContent[i].color = this.selectedColor;
                }
                this.showTooltip = true; // Show the tooltip when text is selected
            }
        },
        changeSelectionColor(color) {
            // Rest of the method remains the same
        },
    },
    mounted() {
        rangy.init();
        this.initializeEditorContent(); // Initialize the editor content when the component is mounted
    },
    computed: {
        editorText() {
            // Replace the <span> elements in the editorContent with the actual text to get the editor's full content
            return this.editorContent.map((item) => item.content).join('');
        },
    },
};
</script>
  
  
<style scoped lang="scss">
.text-editor {
    border: 1px solid #ccc;
    padding: 10px;
    cursor: text;
}

.color-palette-tooltip {
    display: flex;
    justify-content: space-around;
    align-items: center;
}
</style>
  