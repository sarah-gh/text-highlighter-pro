<template>
    <div>
        <div>
            <div v-show="editingText">
                <div class="d-flex w-50 m-auto justify-content-center align-items-center">
                    <input v-model="editedText" class="input form-control" ref="editInput">
                    <button class="btn" @click="applyEditedText">
                        <svg viewBox="0 0 16 16" width="1.3em" height="1.3em" focusable="false" role="img" aria-label="check lg"
                            xmlns="http://www.w3.org/2000/svg" fill="currentColor"
                            class="bi-check-lg mx-4 pointer b-icon bi" data-darkreader-inline-fill=""
                            style="--darkreader-inline-fill: currentColor;">
                            <g>
                                <path
                                    d="M13.485 1.431a1.473 1.473 0 0 1 2.104 2.062l-7.84 9.801a1.473 1.473 0 0 1-2.12.04L.431 8.138a1.473 1.473 0 0 1 2.084-2.083l4.111 4.112 6.82-8.69a.486.486 0 0 1 .04-.045z">
                                </path>
                            </g>
                        </svg>
                    </button>
                </div>
            </div>
            <div v-show="!editingText">
                <div class="d-flex w-50 m-auto justify-content-center align-items-center container-selectable">
                    <h4 class="h4 text-editor selectable" ref="myTextContainer" @mouseup="handleSelection">SELECT ANY PART OF
                        THE TEXT AND SEE WHAT HAPPENS!</h4>
                    <button @click="reset" class="btn reset-button">
                        <svg viewBox="0 0 16 16" width="1.4em" height="1.4em" focusable="false" role="img"
                            aria-label="arrow counterclockwise" xmlns="http://www.w3.org/2000/svg" fill="currentColor"
                            class="bi-arrow-counterclockwise mr-4 pointer b-icon bi" data-darkreader-inline-fill=""
                            style="--darkreader-inline-fill: currentColor;">
                            <g>
                                <path fill-rule="evenodd"
                                    d="M8 3a5 5 0 1 1-4.546 2.914.5.5 0 0 0-.908-.417A6 6 0 1 0 8 2v1z"></path>
                                <path
                                    d="M8 4.466V.534a.25.25 0 0 0-.41-.192L5.23 2.308a.25.25 0 0 0 0 .384l2.36 1.966A.25.25 0 0 0 8 4.466z">
                                </path>
                            </g>
                        </svg>
                    </button>
                    <button @click="changeText" class="btn change-text-button">
                        <svg viewBox="0 0 16 16" width="1.3em" height="1.3em" focusable="false" role="img" aria-label="pen"
                            xmlns="http://www.w3.org/2000/svg" fill="currentColor" class="bi-pen mr-4 pointer b-icon bi"
                            data-darkreader-inline-fill="" style="--darkreader-inline-fill: currentColor;">
                            <g>
                                <path
                                    d="m13.498.795.149-.149a1.207 1.207 0 1 1 1.707 1.708l-.149.148a1.5 1.5 0 0 1-.059 2.059L4.854 14.854a.5.5 0 0 1-.233.131l-4 1a.5.5 0 0 1-.606-.606l1-4a.5.5 0 0 1 .131-.232l9.642-9.642a.5.5 0 0 0-.642.056L6.854 4.854a.5.5 0 1 1-.708-.708L9.44.854A1.5 1.5 0 0 1 11.5.796a1.5 1.5 0 0 1 1.998-.001zm-.644.766a.5.5 0 0 0-.707 0L1.95 11.756l-.764 3.057 3.057-.764L14.44 3.854a.5.5 0 0 0 0-.708l-1.585-1.585z">
                                </path>
                            </g>
                        </svg>
                    </button>
                </div>
            </div>
            <div v-for="id in selectedTextIds" :key="id">
                <b-tooltip custom-class="my-tooltip-class" :target="id" :triggers="getTriggers(id)"
                    :show="isTooltipVisible[id]" @update:show="toggleTooltip(id)">
                    <div class="color-selection">
                        <div class="color-option" v-for="color in colors" :key="color"
                            :style="{ backgroundColor: color, color: color }" @click="changeSelectionColor(color, id)">
                        </div>
                        <div class="color-option" @click="deleteSelectedText(id)">
                            <b-icon-trash-fill class="icon-size"></b-icon-trash-fill>
                        </div>
                    </div>
                </b-tooltip>
            </div>
        </div>
    </div>
</template>
  
<script>
import rangy from "rangy/lib/rangy-core";
import { BTooltip } from "bootstrap-vue";

export default {
    data() {
        return {
            colors: ["rgb(201, 9, 50)", "rgb(186, 194, 78)", "rgb(115, 134, 218)", "rgb(9, 201, 51)"],
            selectedRanges: [],
            selectedTextIds: [],
            selectedColor: "rgb(201, 9, 50)",
            isTooltipVisible: {},
            editingText: false,
            editedText: "",
            oldText: '',
            editingTextContent: '',
        };
    },
    methods: {
        handleSelection() {
            const selectionHandler = () => {
                try {
                    const selection = rangy.getSelection();
                    const text = selection.toString();

                    if (text === "") {
                        // No text is selected, do nothing
                        return;
                    }

                    const range = selection.getRangeAt(0);
                    const { start: whereSelectionStartsInText, end: whereSelectionEndsInText } = range.getBookmark(this.$refs.myTextContainer);

                    // Remove any existing internal tags within the selected text section
                    this.removeInternalTags(range);

                    // Surround the selected text with new tags
                    const tag = document.createElement("span");
                    tag.setAttribute("class", "selected-text");
                    const id = `selected-text-${Math.random().toString(36).substring(7)}`;
                    tag.id = id;
                    this.selectedTextIds.push(id);
                    this.selectedRanges.push(range);
                    this.$set(this.isTooltipVisible, id, false);
                    range.surroundContents(tag);
                } catch (error) {
                    console.log(error);
                }
            };

            // Execute the selection handler asynchronously to allow the DOM to update
            setTimeout(selectionHandler, 400);
        },
        removeInternalTags(range) {
            const containerNode = range.commonAncestorContainer;
            const selectedElements = [];

            // Find all the selected-text spans within the common ancestor container
            const walker = document.createTreeWalker(containerNode, NodeFilter.SHOW_ELEMENT);
            while (walker.nextNode()) {
                const node = walker.currentNode;
                if (node.tagName === "SPAN" && node.classList.contains("selected-text") && range.intersectsNode(node)) {
                    selectedElements.push(node);
                }
            }

            if (selectedElements.length === 0) {
                // No internal selected-text spans found, do nothing
                return;
            }

            // Check if the range contains both an open and closed tag
            if (selectedElements.length >= 2) {
                const firstElement = selectedElements[0];
                const lastElement = selectedElements[selectedElements.length - 1];

                if (firstElement !== lastElement) {
                    // Create a fragment to hold the content of the selected elements
                    const contentFragment = document.createDocumentFragment();
                    const selectedContentRange = rangy.createRange();
                    selectedContentRange.setStartAfter(firstElement);
                    selectedContentRange.setEndBefore(lastElement);
                    contentFragment.appendChild(selectedContentRange.cloneContents());

                    // Remove the internal selected-text spans
                    for (const element of selectedElements) {
                        const parent = element.parentNode;
                        parent.removeChild(element);
                    }

                    // Surround the selected content with new tags
                    const tag = document.createElement("span");
                    tag.setAttribute("class", "selected-text");
                    const id = `selected-text-${Math.random().toString(36).substring(7)}`;
                    tag.id = id;
                    this.selectedTextIds.push(id);
                    this.selectedRanges.push(range);
                    this.$set(this.isTooltipVisible, id, false);
                    tag.appendChild(contentFragment);

                    // Insert the new tag back into the DOM
                    range.insertNode(tag);
                    return;
                }
            }

            // Remove internal selected-text spans
            for (const element of selectedElements) {
                const elementRange = rangy.createRange();
                elementRange.selectNode(element);

                if (!range.equals(elementRange)) {
                    const parent = element.parentNode;
                    const content = element.firstChild; // Get the first child (text node) of the element
                    parent.replaceChild(content, element);
                }
            }
        },
        deleteSelectedText(id) {
            const index = this.selectedTextIds.indexOf(id);
            if (index !== -1) {
                const selectedElement = document.getElementById(id);
                if (selectedElement) {
                    const text = selectedElement.innerText;
                    selectedElement.outerHTML = text;
                    this.selectedTextIds.splice(index, 1);
                    this.selectedRanges.splice(index, 1);
                    this.$delete(this.isTooltipVisible, id);
                }
            }
        },
        reset() {
            for (const id of this.selectedTextIds) {
                const selectedElement = document.getElementById(id);
                if (selectedElement) {
                    const text = selectedElement.innerText;
                    selectedElement.outerHTML = text;
                }
            }
            this.selectedTextIds = [];
            this.selectedRanges = [];
            this.isTooltipVisible = {};
        },
        changeText() {
            this.editingText = true;
            this.oldText = this.$refs.myTextContainer.innerText;
            this.editingTextContent = this.$refs.myTextContainer.innerHTML;
            this.editedText = this.$refs.myTextContainer.innerText;
        },

        applyEditedText() {
            this.editingText = false;
            const editedText = this.editedText.trim();
            if (editedText == this.oldText) {
                this.$refs.myTextContainer.innerHTML = this.editingTextContent;
                this.editedText = "";
                return;
            }
            if (editedText) {
                this.$refs.myTextContainer.innerText = editedText;
            }
            this.editedText = "";
        },

        changeSelectionColor(color, id) {
            const selectedElement = document.getElementById(id);
            if (selectedElement) {
                selectedElement.style.backgroundColor = color;
                this.selectedColor = color;
            }
        },
        toggleTooltip(id) {
            this.$set(this.isTooltipVisible, id, !this.isTooltipVisible[id]);
        },
        getTriggers(id) {
            return this.selectedTextIds.length === 1 && this.selectedTextIds[0] === id ? "hover" : "hover";
        }
    },
    mounted() {
        rangy.init();
    },
    components: {
        BTooltip,
    },
};
</script>
  
<style lang="scss">
.my-tooltip-class {
    width: fit-content;
    margin-top: -15px !important;

    .tooltip-inner {
        width: fit-content;
    }
}

.icon-size {
    font-size: 24px;
}

.container-selectable {
    border: 1px solid #ccc;
    padding: 10px;
    .selectable {
        width: fit-content;
        cursor: text;
    }
}


.actions {
    display: flex;
    justify-content: space-around;
    align-items: center;
}

.color-selection {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.color-option {
    width: 30px;
    height: 30px;
    border-radius: 5px;
    margin: 4px 3px;
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
}

.selected-text {
    background-color: rgb(201, 9, 50);
    cursor: pointer;
    padding: 4px 5px;
    border-radius: 4px;
}</style>
  