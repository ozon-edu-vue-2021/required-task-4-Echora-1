<template>
  <div v-click-outside="hideDropdown" class="wrapper">
    <label :for="id" class="base-label">
      {{ label }}
    </label>
    <div
        :class="['base-input-wrapper', {'base-input-wrapper_focused': focused}]"
    >
      <input
          :id="id"
          class="base-input"
          v-bind="$attrs"
          :placeholder="$attrs.placeholder"
          @focus="focusHandler"
          @input="inputHandler"
          @blur="blurHandler"
          v-model="searchValue"
      >
    </div>
    <div
        v-if="isDropdownOpen"
        class="list-selector___dropdown"
    >
      <ul>
        <li
            v-for="item in filteredList"
            :key="item.id"
            @click="onItemClick(item)"
        >
          {{ item[itemKey] }}
        </li>
        <li v-if="!filteredList.length" class="empty">
            Ничего не найдено
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import ClickOutside from "vue-click-outside";
import {debounce}  from "../helpers/debounce";

export default {
  props: {
    value: {
      type: String,
      default: ''
    },

    id: {
      type: String,
      default: ''
    },

    label: {
      type: String,
      default: ''
    },

    list: {
      type: Array
    },

    itemKey: {
      type: String
    }
  },

  directives: {
    ClickOutside,
  },

  data() {
    return {
      focused: false,
      isDropdownOpen: false,
      selectedListItem: {},
      filteredList: this.list,
      searchValue: '',
      debouncedSearchItem: null,
    }
  },

  methods: {
    inputHandler() {
      this.isDropdownOpen = true
    },

    focusHandler() {
      this.focused = true
      this.isDropdownOpen = true
    },

    blurHandler() {
      this.focused = false
    },

    hideDropdown() {
      this.isDropdownOpen = false
    },

    onItemClick(item) {
      this.selectedListItem = item
      this.searchValue = item[this.itemKey]
      this.isDropdownOpen = false
    },

    getSearchItem() {
      return this.filteredList = this.list.filter(item => item[this.itemKey].toLowerCase().includes(this.searchValue.toLowerCase()))
    }
  },

  created() {
    this.debouncedSearchItem = debounce(this.getSearchItem, 500);
  },


  watch: {
    searchValue() {
      this.debouncedSearchItem()
    },

    selectedListItem() {
      this.$emit('selected', this.selectedListItem)
    }
  }
}
</script>

<style scoped>

.wrapper {
  position: relative;
}

.base-input-wrapper {
  border: 3px solid #B0B6BE;
  border-radius: 4px;
  width: 100%;
  margin-top: 8px;
}

.base-input-wrapper_focused {
  border-color: gray;
}

.base-input {
  width: 100%;
  height: 30px;
  margin: 0;
  border: none;
  outline: none;
  padding: 3px 26px 3px 6px;
  background-color: transparent;
  background-image: url("../assets/images/triangle.png");
  background-repeat: no-repeat;
  background-position: center right 8px;
  background-size: 15px;
}

.base-label {
  color: #6B7686;
  font-size: 18px;
}

.list-selector___dropdown {
  max-height: 100px;
  width: -webkit-fill-available;
  overflow-y: scroll;
  position: absolute;
  background-color: white;
  border: 1px solid gray;
  border-radius: 4px;
  margin-top: 2px;
  z-index: 1;
}

.list-selector___dropdown ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

.list-selector___dropdown li {
  padding: 6px;
  cursor: pointer;
}

.list-selector___dropdown li:hover {
  background-color: #B0B6BE66;
}

.empty {
  text-align: center;
  font-size: 12px;
}

li.empty:hover {
  background-color: transparent;
}

</style>