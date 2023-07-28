<template>
  <div>
    <div>
      <h3>Text Editor</h3>
      <div class="text-editor selectable" ref="myTextContainer" @mouseup="handleSelection">
        SELECT ANY PART OF THE TEXT AND SEE WHAT HAPPENS!
      </div>
      <div v-if="idSelection2" >
        <b-tooltip custom-class="my-tooltip-class" :target="idSelection2" triggers="click" :show.sync="tooltipVisibility">
          <div class="color-selection">
            <div class="color-option" v-for="color in colors" :key="color" :style="{ backgroundColor: color, color: color}"
              @click="changeSelectionColor(color)">cccic</div>
          </div>
        </b-tooltip>
      </div>
    </div>
  </div>
</template>

<script>
import rangy from 'rangy/lib/rangy-core';
import { BTooltip } from 'bootstrap-vue';

export default {
  data() {
    return {
      colors: ['rgb(201, 9, 50)', 'rgb(186, 194, 78)', 'rgb(115, 134, 218)', 'rgb(9, 201, 51)'],
      selectedRanges: [],
      idSelection: 0,
      selectedColor: 'rgb(201, 9, 50)',
      tooltipVisibility: false, // Initialize tooltip visibility to false
    };
  },
  methods: {
    handleSelection() {
      this.tooltipVisibility = false;
      setTimeout(() => {
        try {
          console.log('handleSelection');
          const range = rangy.getSelection().getRangeAt(0);

          for (const previousRange of this.selectedRanges) {
            if (previousRange.intersects(range)) {
              return;
            }
          }

          const tag = document.createElement('span');
          tag.setAttribute('class', 'selected-text');
          const id = `selected-text-${Math.random().toString(36).substring(7)}`
          tag.id = id;

          const selectedText = range.toString();
          if (selectedText === '') {
            return;
          }

          this.idSelection = id;
          range.surroundContents(tag);
          tag.addEventListener('click', (event) => this.changeSelectionId(event, id));
          this.tooltipVisibility = true;
        } catch (error) {
          console.log(error);
        }
      }, 500);
    },
    changeSelectionId(event, id) {
      console.log('changeSelectionId');
      event.stopPropagation();
      this.idSelection = id;
      this.tooltipVisibility = !this.tooltipVisibility; // Show the tooltip by setting tooltipVisibility to true
    },
    changeSelectionColor(color) {
      console.log('changeSelectionColor');
      const selectedElement = document.getElementById(this.idSelection);
      if (selectedElement) {
        selectedElement.style.backgroundColor = color;
        this.selectedColor = color; // Update the selectedColor to the newly chosen color
      }
      // for (const range of this.selectedRanges) {
      //   const span = range.getClientRects()[0].querySelector('.selected-text');
      //   span.style.backgroundColor = color;
      // }
      // this.selectedRanges = [];
      // this.selectedColor = color;
    },
  },
  computed: {
    idSelection2() {
      return this.idSelection
    }
  },
  mounted() {
    rangy.init();
  },
};
</script>

<style lang="scss">
.my-tooltip-class {
  width: fit-content;
  margin-top: -15px !important;
  opacity: 1 !important;
  .tooltip-inner {
    width: fit-content;
  }
}
.selectable {
  width: fit-content;
  margin: 0 auto;
  border: 1px solid #ccc;
  padding: 10px;
  cursor: text;
}

.actions {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.color-selection {
  // position: absolute;
  // top: 0;
  // left: 0;
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
}

.selected-text {
  // border: 1px solid #000;
  background-color: #ccc;
  cursor: pointer;
  padding: 4px 5px;
  border-radius: 4px;
}
</style>

<style>
.was-validated .custom-range:valid ~ .valid-feedback,
.was-validated .custom-range:valid ~ .valid-tooltip, .custom-range.is-valid ~ .valid-feedback,
.custom-range.is-valid ~ .valid-tooltip {
  display: block;
}
.tooltip.b-tooltip {
  display: block;
  opacity: 0.9;
  outline: 0;
}
.tooltip.b-tooltip.fade:not(.show) {
  opacity: 0;
}
.tooltip.b-tooltip.show {
  opacity: 0.9;
}
.tooltip.b-tooltip.noninteractive {
  pointer-events: none;
}
.tooltip.b-tooltip .arrow {
  margin: 0 0.25rem;
}
.tooltip.b-tooltip.bs-tooltip-right .arrow, .tooltip.b-tooltip.b-tooltip-dark.bs-tooltip-auto[x-placement^=right] .arrow, .tooltip.b-tooltip.b-tooltip-light.bs-tooltip-auto[x-placement^=right] .arrow, .tooltip.b-tooltip.b-tooltip-danger.bs-tooltip-auto[x-placement^=right] .arrow, .tooltip.b-tooltip.b-tooltip-warning.bs-tooltip-auto[x-placement^=right] .arrow, .tooltip.b-tooltip.b-tooltip-info.bs-tooltip-auto[x-placement^=right] .arrow, .tooltip.b-tooltip.b-tooltip-success.bs-tooltip-auto[x-placement^=right] .arrow, .tooltip.b-tooltip.b-tooltip-secondary.bs-tooltip-auto[x-placement^=right] .arrow, .tooltip.b-tooltip.b-tooltip-primary.bs-tooltip-auto[x-placement^=right] .arrow, .tooltip.b-tooltip.bs-tooltip-left .arrow, .tooltip.b-tooltip.b-tooltip-dark.bs-tooltip-auto[x-placement^=left] .arrow, .tooltip.b-tooltip.b-tooltip-light.bs-tooltip-auto[x-placement^=left] .arrow, .tooltip.b-tooltip.b-tooltip-danger.bs-tooltip-auto[x-placement^=left] .arrow, .tooltip.b-tooltip.b-tooltip-warning.bs-tooltip-auto[x-placement^=left] .arrow, .tooltip.b-tooltip.b-tooltip-info.bs-tooltip-auto[x-placement^=left] .arrow, .tooltip.b-tooltip.b-tooltip-success.bs-tooltip-auto[x-placement^=left] .arrow, .tooltip.b-tooltip.b-tooltip-secondary.bs-tooltip-auto[x-placement^=left] .arrow, .tooltip.b-tooltip.b-tooltip-primary.bs-tooltip-auto[x-placement^=left] .arrow {
  margin: 0.25rem 0;
}

.tooltip.b-tooltip-primary.bs-tooltip-top .arrow::before, .tooltip.b-tooltip-primary.bs-tooltip-auto[x-placement^=top] .arrow::before {
  border-top-color: #007bff;
}
.tooltip.b-tooltip-primary.bs-tooltip-right .arrow::before, .tooltip.b-tooltip-primary.bs-tooltip-auto[x-placement^=right] .arrow::before {
  border-right-color: #007bff;
}
.tooltip.b-tooltip-primary.bs-tooltip-bottom .arrow::before, .tooltip.b-tooltip-primary.bs-tooltip-auto[x-placement^=bottom] .arrow::before {
  border-bottom-color: #007bff;
}
.tooltip.b-tooltip-primary.bs-tooltip-left .arrow::before, .tooltip.b-tooltip-primary.bs-tooltip-auto[x-placement^=left] .arrow::before {
  border-left-color: #007bff;
}
.tooltip.b-tooltip-primary .tooltip-inner {
  color: #fff;
  background-color: #007bff;
}

.tooltip.b-tooltip-secondary.bs-tooltip-top .arrow::before, .tooltip.b-tooltip-secondary.bs-tooltip-auto[x-placement^=top] .arrow::before {
  border-top-color: #6c757d;
}
.tooltip.b-tooltip-secondary.bs-tooltip-right .arrow::before, .tooltip.b-tooltip-secondary.bs-tooltip-auto[x-placement^=right] .arrow::before {
  border-right-color: #6c757d;
}
.tooltip.b-tooltip-secondary.bs-tooltip-bottom .arrow::before, .tooltip.b-tooltip-secondary.bs-tooltip-auto[x-placement^=bottom] .arrow::before {
  border-bottom-color: #6c757d;
}
.tooltip.b-tooltip-secondary.bs-tooltip-left .arrow::before, .tooltip.b-tooltip-secondary.bs-tooltip-auto[x-placement^=left] .arrow::before {
  border-left-color: #6c757d;
}
.tooltip.b-tooltip-secondary .tooltip-inner {
  color: #fff;
  background-color: #6c757d;
}

.tooltip.b-tooltip-success.bs-tooltip-top .arrow::before, .tooltip.b-tooltip-success.bs-tooltip-auto[x-placement^=top] .arrow::before {
  border-top-color: #28a745;
}
.tooltip.b-tooltip-success.bs-tooltip-right .arrow::before, .tooltip.b-tooltip-success.bs-tooltip-auto[x-placement^=right] .arrow::before {
  border-right-color: #28a745;
}
.tooltip.b-tooltip-success.bs-tooltip-bottom .arrow::before, .tooltip.b-tooltip-success.bs-tooltip-auto[x-placement^=bottom] .arrow::before {
  border-bottom-color: #28a745;
}
.tooltip.b-tooltip-success.bs-tooltip-left .arrow::before, .tooltip.b-tooltip-success.bs-tooltip-auto[x-placement^=left] .arrow::before {
  border-left-color: #28a745;
}
.tooltip.b-tooltip-success .tooltip-inner {
  color: #fff;
  background-color: #28a745;
}

.tooltip.b-tooltip-info.bs-tooltip-top .arrow::before, .tooltip.b-tooltip-info.bs-tooltip-auto[x-placement^=top] .arrow::before {
  border-top-color: #17a2b8;
}
.tooltip.b-tooltip-info.bs-tooltip-right .arrow::before, .tooltip.b-tooltip-info.bs-tooltip-auto[x-placement^=right] .arrow::before {
  border-right-color: #17a2b8;
}
.tooltip.b-tooltip-info.bs-tooltip-bottom .arrow::before, .tooltip.b-tooltip-info.bs-tooltip-auto[x-placement^=bottom] .arrow::before {
  border-bottom-color: #17a2b8;
}
.tooltip.b-tooltip-info.bs-tooltip-left .arrow::before, .tooltip.b-tooltip-info.bs-tooltip-auto[x-placement^=left] .arrow::before {
  border-left-color: #17a2b8;
}
.tooltip.b-tooltip-info .tooltip-inner {
  color: #fff;
  background-color: #17a2b8;
}

.tooltip.b-tooltip-warning.bs-tooltip-top .arrow::before, .tooltip.b-tooltip-warning.bs-tooltip-auto[x-placement^=top] .arrow::before {
  border-top-color: #ffc107;
}
.tooltip.b-tooltip-warning.bs-tooltip-right .arrow::before, .tooltip.b-tooltip-warning.bs-tooltip-auto[x-placement^=right] .arrow::before {
  border-right-color: #ffc107;
}
.tooltip.b-tooltip-warning.bs-tooltip-bottom .arrow::before, .tooltip.b-tooltip-warning.bs-tooltip-auto[x-placement^=bottom] .arrow::before {
  border-bottom-color: #ffc107;
}
.tooltip.b-tooltip-warning.bs-tooltip-left .arrow::before, .tooltip.b-tooltip-warning.bs-tooltip-auto[x-placement^=left] .arrow::before {
  border-left-color: #ffc107;
}
.tooltip.b-tooltip-warning .tooltip-inner {
  color: #212529;
  background-color: #ffc107;
}

.tooltip.b-tooltip-danger.bs-tooltip-top .arrow::before, .tooltip.b-tooltip-danger.bs-tooltip-auto[x-placement^=top] .arrow::before {
  border-top-color: #dc3545;
}
.tooltip.b-tooltip-danger.bs-tooltip-right .arrow::before, .tooltip.b-tooltip-danger.bs-tooltip-auto[x-placement^=right] .arrow::before {
  border-right-color: #dc3545;
}
.tooltip.b-tooltip-danger.bs-tooltip-bottom .arrow::before, .tooltip.b-tooltip-danger.bs-tooltip-auto[x-placement^=bottom] .arrow::before {
  border-bottom-color: #dc3545;
}
.tooltip.b-tooltip-danger.bs-tooltip-left .arrow::before, .tooltip.b-tooltip-danger.bs-tooltip-auto[x-placement^=left] .arrow::before {
  border-left-color: #dc3545;
}
.tooltip.b-tooltip-danger .tooltip-inner {
  color: #fff;
  background-color: #dc3545;
}

.tooltip.b-tooltip-light.bs-tooltip-top .arrow::before, .tooltip.b-tooltip-light.bs-tooltip-auto[x-placement^=top] .arrow::before {
  border-top-color: #f8f9fa;
}
.tooltip.b-tooltip-light.bs-tooltip-right .arrow::before, .tooltip.b-tooltip-light.bs-tooltip-auto[x-placement^=right] .arrow::before {
  border-right-color: #f8f9fa;
}
.tooltip.b-tooltip-light.bs-tooltip-bottom .arrow::before, .tooltip.b-tooltip-light.bs-tooltip-auto[x-placement^=bottom] .arrow::before {
  border-bottom-color: #f8f9fa;
}
.tooltip.b-tooltip-light.bs-tooltip-left .arrow::before, .tooltip.b-tooltip-light.bs-tooltip-auto[x-placement^=left] .arrow::before {
  border-left-color: #f8f9fa;
}
.tooltip.b-tooltip-light .tooltip-inner {
  color: #212529;
  background-color: #f8f9fa;
}

.tooltip.b-tooltip-dark.bs-tooltip-top .arrow::before, .tooltip.b-tooltip-dark.bs-tooltip-auto[x-placement^=top] .arrow::before {
  border-top-color: #343a40;
}
.tooltip.b-tooltip-dark.bs-tooltip-right .arrow::before, .tooltip.b-tooltip-dark.bs-tooltip-auto[x-placement^=right] .arrow::before {
  border-right-color: #343a40;
}
.tooltip.b-tooltip-dark.bs-tooltip-bottom .arrow::before, .tooltip.b-tooltip-dark.bs-tooltip-auto[x-placement^=bottom] .arrow::before {
  border-bottom-color: #343a40;
}
.tooltip.b-tooltip-dark.bs-tooltip-left .arrow::before, .tooltip.b-tooltip-dark.bs-tooltip-auto[x-placement^=left] .arrow::before {
  border-left-color: #343a40;
}
.tooltip.b-tooltip-dark .tooltip-inner {
  color: #fff;
  background-color: #343a40;
}
/*  */
.bs-tooltip-auto[x-placement^=top] .arrow, .bs-tooltip-top .arrow {
    bottom: 0;
}
.tooltip .arrow {
    position: absolute;
    display: block;
    width: 0.8rem;
    height: 0.4rem;
}
.tooltip.b-tooltip-danger.bs-tooltip-auto[x-placement^=top] .arrow:before, .tooltip.b-tooltip-danger.bs-tooltip-top .arrow:before {
    border-top-color: #dc3545;
}
.bs-tooltip-auto[x-placement^=top] .arrow:before, .bs-tooltip-top .arrow:before {
    top: 0;
    border-width: 0.4rem 0.4rem 0;
    border-top-color: #000;
}
.tooltip .arrow:before {
    position: absolute;
    content: "";
    border-color: transparent;
    border-style: solid;
}

.bs-tooltip-auto[x-placement^=top] .arrow:before, .bs-tooltip-top .arrow:before {
    top: 5px;
    z-index: 100;
    border-width: 0.4rem 0.4rem 0;
    border-top-color: #000 !important;
}
</style>
