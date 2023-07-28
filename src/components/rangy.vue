<template>
    <div>
        <div>
            <h3>Text Editor</h3>
            <div class="text-editor selectable" ref="myTextContainer" @mouseup="handleSelection">
                Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed tincidunt tellus nec ex hendrerit, quis
                scelerisque odio semper.
            </div>
        </div>
        <div>
            <h3>Color Palette</h3>
            <div class="color-palette actions">
                <div class="color-option" v-for="color in colors" :key="color" :style="{ backgroundColor: color }"
                    @click="changeSelectionColor(color)">
                    {{ color }}
                </div>
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
        };
    },
    methods: {
        handleSelection() {
            const selection = rangy.getSelection();
            if (!selection.isCollapsed) {
                this.selectedRanges.push(selection.getRangeAt(0));
            }
        },
        changeSelectionColor(color) {
            for (const range of this.selectedRanges) {
                const span = document.createElement('span');
                span.style.backgroundColor = color;
                range.surroundContents(span);
            }
            this.selectedRanges = [];
            this.selectedColor = color; // Update the selectedColor to the newly chosen color
        },
    },
    mounted() {
        rangy.init();
    },
};
</script>
  
<style scoped lang="scss">
.selectable {
    border: 1px solid #ccc;
    padding: 10px;
    cursor: text;
}

.actions {
    display: flex;
    justify-content: space-around;
    align-items: center;
}
</style>
  