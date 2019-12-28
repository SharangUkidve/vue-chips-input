/* eslint-disable vue/no-duplicate-attributes */
<template>
  <div class="wrapper" @click.self="focusOnInput">
    <div class="chips-container">
      <div
        :class="{
          [chipClass]: !!chipClass,
          [invalidClass || 'invalid']: chip.invalid,
          disabled: disabled
        }"
        class="chip"
        v-for="(chip, index) of chips"
        :key="chip.value || chip"
        :tabindex="disabled || !removable ? undefined : 0"
        @keyup.delete="removeChip(index)"
      >
        <span class="chip-text">
          <slot :chip="chip" :index="index">{{ chip.value || chip }}</slot>
        </span>
        <button
          class="button-chip-remove"
          v-if="removable && !disabled"
          @click="removeChip(index)"
          tabindex="-1"
        >
          <slot name="removeIcon">
            <i>&times;</i>
          </slot>
        </button>
      </div>
      <input
        type="text"
        class="chip-input"
        ref="newChipInput"
        v-if="
          !disabled && ((!!maxChips && chips.length < maxChips) || !maxChips)
        "
        :value="newChip"
        :id="$attrs.id"
        :name="$attrs.name"
        :placeholder="$attrs.placeholder"
        :disabled="disabled"
        @input="updateNewChipModel($event)"
        @keyup.esc="resetNewChipModel()"
        @keyup.enter="addNewChips($event)"
        @keydown.tab="addNewChipsOnBlur($event)"
        @keyup.delete="checkAndRemovePrevious()"
      />
    </div>
  </div>
</template>

<script>
import cloneDeep from "lodash/cloneDeep";
export default {
  inheritAttrs: false,
  props: {
    value: {
      type: Array,
      required: true
    },
    disabled: {
      type: Boolean,
      default: false
    },
    removable: {
      type: Boolean,
      default: true
    },
    addOnBlur: {
      type: Boolean,
      default: false
    },
    splitOn: {
      type: String,
      default: ","
    },
    maxChipLength: Number,
    minChipLength: {
      type: Number,
      default: 0
    },
    maxChips: Number,
    invalidClass: String,
    chipClass: String
  },
  data() {
    return {
      chips: [],
      newChip: "",
      allowEmptyDelete: true,
      lowerChips: []
    };
  },
  created() {
    this.setModel(this.value);
  },
  methods: {
    addNewChips() {
      if (!this.newChip || this.chips.length >= this.maxChips) {
        return;
      }
      const newChipsArray = this.newChip.trim().split(this.splitOn);
      const possibleAdditions = this.maxChips
        ? Math.min(this.maxChips - this.chips.length, newChipsArray.length)
        : newChipsArray.length;
      const addedChips = [];
      for (let i = 0; i < possibleAdditions; i++) {
        const newChip = newChipsArray[i].trim();
        if (
          (this.minChipLength >= 0 && newChip.length < this.minChipLength) ||
          (this.maxChipLength && newChip.length > this.maxChipLength)
        ) {
          continue;
        }
        const newChipLower = newChip.toLowerCase();
        if (
          newChipLower.length &&
          this.lowerChips.findIndex(
            chip => chip.value === newChipLower || chip === newChipLower
          ) === -1
        ) {
          addedChips.push(newChip);
          this.chips.push(newChip);
          this.lowerChips.push(newChipLower);
        }
      }
      if (addedChips.length) {
        this.emitAdd(addedChips);
        this.emitModelChange();
      }
      this.resetNewChipModel();
    },
    addNewChipsOnBlur() {
      if (this.addOnBlur && this.newChip.length) {
        this.addNewChips();
        this.focusOnInput();
      }
    },
    checkAndRemovePrevious() {
      if (!this.removable) {
        return;
      }
      if (this.newChip.length === 0) {
        if (this.allowEmptyDelete) {
          this.removeChip();
        } else {
          this.allowEmptyDelete = true;
        }
      }
    },
    emitAdd(newChips) {
      this.$emit("add", newChips);
    },
    emitModelChange() {
      this.$emit("input", this.chips);
    },
    emitRemove(chip, index) {
      this.$emit("remove", chip, index);
    },
    focusOnInput() {
      if (this.$refs.newChipInput) {
        this.$refs.newChipInput.focus();
      }
    },
    removeChip(index = this.chips.length - 1) {
      if (!this.removable || this.disabled) {
        return;
      }
      if (this.chips.length) {
        const removed = this.chips.splice(index, 1);
        this.lowerChips.splice(index);
        this.emitModelChange();
        this.emitRemove(removed, index);
      }
    },
    resetNewChipModel() {
      this.newChip = "";
      this.allowEmptyDelete = true;
    },
    setModel(newValue) {
      this.chips = cloneDeep(newValue);
      this.lowerChips = this.chips.map(chip =>
        chip.value ? chip.value.toLowerCase() : chip.toLowerCase()
      );
    },
    updateNewChipModel($event) {
      this.newChip = $event.target.value;
      if (this.newChip.length) {
        this.allowEmptyDelete = false;
      }
    }
  },
  watch: {
    value: function(newValue) {
      // Make sure new Array is actually different
      if (newValue && newValue !== this.chips) {
        this.setModel(newValue);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
* {
  box-sizing: border-box;
  font-size: inherit;
}

.wrapper {
  border: 1px solid rgba(0, 0, 0, 0.15);
  padding: 4px;
  border-radius: 8px;
}

.chips-container {
  display: flex;
  flex-wrap: wrap;
}

.chip {
  margin: 4px;
  display: flex;
  align-items: center;
  padding: 6px 8px;
  border-radius: 10px;
  background-color: #0c4dc7;
  color: #ffffff;
  border-radius: 100px;
  cursor: default;

  &:focus,
  &:hover {
    background-color: #0c4d87;
  }

  &.disabled {
    background-color: #0c2077;
  }

  &.invalid {
    background-color: #dd0036;

    &:focus,
    &:hover {
      background-color: #aa0036;
    }

    &.disabled {
      background-color: #770036;
    }
  }
}

.button-chip-remove {
  padding: 0;
  margin: 0 0 0 8px;
  background-color: transparent;
  border: none;
  line-height: 1;
  cursor: pointer;
  outline: none;

  i {
    font-size: 16px;
    border-radius: 50%;
    color: red;
    background-color: #ffffff;
    display: block;
    width: 16px;
    height: 16px;
    font-style: normal;
  }
}

.chip-input {
  border: none;
  flex: 1;
  outline: none;
  padding: 4px;
  margin: 4px;
  min-width: 100px;
}
</style>
