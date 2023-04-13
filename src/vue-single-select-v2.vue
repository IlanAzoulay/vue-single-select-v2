<template>
  <div ref="vuesingleselect">
    <div v-if="!selectedOption" :class="classes.wrapper">
      <div class="wrapper-child">
        <input
          ref="search"
          :disabled="disabled"
          :class="[classes.input, isRequired]"
          :style="getStyle()"
          @click="toggleDropdown"
          @keydown.enter.prevent="setOption"
          @keyup.enter.prevent="setOption"
          @keyup.down="movePointerDown"
          @keyup.tab.stop="closeOut"
          @keyup.esc.stop="closeOut"
          @keyup.up="movePointerUp"
          @focus="inFocus = true"
          @blur="inFocus = false"
          :placeholder="placeholder"
          autocomplete="off"
          :required="required"
          v-model="searchText"
        />

        <!-- Arrow icon -->
        <div
          @click="toggleDropdown"
          :class="[classes.icons]"
          class="icon-arrow"
          :style="dropdownOpen? 'rotate: 180deg' : ''"
          
        >
          <svg aria-hidden="true" viewBox="0 0 448 512" :style="{'font-size': iconSize}">
            <path
              d="M207.029 381.476L12.686 187.132c-9.373-9.373-9.373-24.569 0-33.941l22.667-22.667c9.357-9.357 24.522-9.375 33.901-.04L224 284.505l154.745-154.021c9.379-9.335 24.544-9.317 33.901.04l22.667 22.667c9.373 9.373 9.373 24.569 0 33.941L240.971 381.476c-9.373 9.372-24.569 9.372-33.942 0z"
            />
          </svg>
        </div>

        <!-- Dropped down content -->
        <ul
          tabindex="-1"
          ref="options"
          v-show="dropdownOpen"
          :style="{'max-height': maxHeight}"
          style="z-index: 100;"
          :class="[classes.dropdown]"
        >
          <li
            tabindex="-1"
            v-for="(option, idx) in matchingOptions"
            :key="idx"
            :class="idx === pointer ? classes.activeClass : ''"
            @mouseover="setPointerIdx(idx)"
            @keyup.enter="setOption"
            @keyup.up="movePointerUp"
            @keyup.down="movePointerDown"
            @click.prevent="setOption"
          >
            <slot name="option" v-bind="{option,idx}">{{ option }}</slot>
          </li>
        </ul>
      </div>
    </div>

    <!-- Option selected -->
    <div :class="classes.wrapper" v-if="selectedOption">
      <input
        :class="[classes.input]"
        :style="getStyle()"
        ref="match"
        :required="required"
        @input="switchToSearch($event)"
        :value="selectedOption"
      />
      <input type="hidden" :name="name" ref="selectedValue" :value="selectedOption" />
      
      <!-- Closing X icon -->
      <div class="icon-close" :class="classes.icons" >
        <svg aria-hidden="true" viewBox="0 0 512 512" :style="{'cursor': 'pointer', 'font-size': iconSize}" @click="closeOut">
          <path
            d="M256 8C119 8 8 119 8 256s111 248 248 248 248-111 248-248S393 8 256 8zm121.6 313.1c4.7 4.7 4.7 12.3 0 17L338 377.6c-4.7 4.7-12.3 4.7-17 0L256 312l-65.1 65.6c-4.7 4.7-12.3 4.7-17 0L134.4 338c-4.7-4.7-4.7-12.3 0-17l65.6-65-65.6-65.1c-4.7-4.7-4.7-12.3 0-17l39.6-39.6c4.7-4.7 12.3-4.7 17 0l65 65.7 65.1-65.6c4.7-4.7 12.3-4.7 17 0l39.6 39.6c4.7 4.7 4.7 12.3 0 17L312 256l65.6 65.1z"
          />
        </svg>
      </div>

    </div>
  </div>
</template>
  
  
<script>
import pointerScroll from './pointerScroll';

export default {
  props: {
    value: {
      required: true
    },
    name: {
      type: String,
      required: false,
      default: () => ''
    },
    options: {
      type: Array,
      required: false,
      default: () => []
    },
    placeholder: {
      type: String,
      required: false,
      default: () => 'Search Here'
    },
    componentHeight: {
      type: String,
      default: '40px',
      required: false
    },
    backgroundColor: {
      type: String,
      default: '#fff',
      required: false
    },
    iconSize: {
      type: String,
      default: '20px',
      required: false
    },
    maxHeight: {
      type: String,
      default: () => '220px',
      required: false
    },
    classes: {
      type: Object,
      required: false,
      default: () => {
        return {
          pointer: -1,
          wrapper: 'single-select-wrapper',
          input: 'search-input',
          icons: 'icons',
          required: 'required',
          activeClass: 'active',
          dropdown: 'dropdown'
        };
      }
    },
    initial: {
      type: String,
      required: false,
      default: () => null
    },
    disabled: {
      type: Boolean,
      required: false,
      default: () => false
    },
    required: {
      type: Boolean,
      required: false,
      default: () => false
    },
    index: {
      type: Number,
      required: false,
      default: 0
    }
  },

  mixins: [pointerScroll],

  mounted() {
    document.addEventListener('click', this.handleClickOutside);
    document.addEventListener('keyup', this.handleKeyOutside);
    if (this.value && this.options.includes(this.value)) {
      this.selectedOption = this.value;
      return;
    }
    // this.searchText = this.initial;
    this.selectedOption = this.initial;
  },
  unmounted() {
    document.removeEventListener('keyup', this.handleKeyOutside);
    document.removeEventListener('click', this.handleClickOutside);
  },

  data() {
    return {
      searchText: null,
      selectedOption: null,
      dropdownOpen: false,
      inFocus: false
    };
  },

  watch: {
    value(curr) {
      this.selectedOption = curr;
    },
    searchText(curr, prev) {
      if (curr !== prev) {
        this.pointer = -1;
      }
      if (curr){
        if (!this.dropdownOpen && curr.length > 0){
          this.dropdownOpen = true;
        }
      }
    },
    selectedOption(curr) {
      this.$emit('input', curr);
    },
    dropdownOpen(curr, prev) {
      if (curr === prev) {
        return;
      }
      if (!curr) {
        this.searchText = null;
        return;
      }

      if (!this.searchText) {
        this.searchText = '';
      }

      this.$nextTick().then(() => {
        this.$refs.search.focus();
      });
    }
  },

  computed: {
    isRequired() {
      if (!this.required) {
        return '';
      }
      if (this.selectedOption) {
        return '';
      }
      return 'required';
    },
    matchingOptions() {
      if (this.searchText === null) {
        return null;
      }
      if (!this.searchText.length) {
        return [...this.options];
      }
      return this.options.filter(option => this.filterStart(option));
    }
  },

  methods: {
    // First letter match filter
    filterStart(option){
      return option.toString().substring(0, this.searchText.length).toLowerCase() === this.searchText.toString().toLowerCase();
    },
    // search engine filter
    filterSearch(option) {
      return option.toString().toLowerCase().includes(this.searchText.toString().toLowerCase());
    },
    setPointerIdx(idx) {
      this.pointer = idx;
    },
    seedSearchText() {
      if (this.searchText !== null) {
        return;
      }
      this.searchText = '';
    },
    switchToSearch(event) {
      this.$refs.selectedValue.value = null;
      this.searchText = event.target.value;
      this.selectedOption = null;
      this.dropdownOpen = true;
    },
    toggleDropdown() {
      if (this.disabled) {return;}
      this.dropdownOpen = !this.dropdownOpen;
      if (this.dropdownOpen){
        this.seedSearchText();
      }
    },
    closeOut() {
      this.selectedOption = null;
      this.dropdownOpen = false;
      this.searchText = null;
    },
    movePointerDown() {
      if (!this.matchingOptions) {
        return;
      }
      if (this.pointer >= this.matchingOptions.length - 1) {
        return;
      }

      this.pointer++;
    },
    movePointerUp() {
      if (this.pointer > 0) {
        this.pointer--;
      }
    },
    setOption() {
      if (!this.matchingOptions || !this.matchingOptions.length) {
        return;
      }

      if (this.pointer === -1) {
        this.pointer++;
      }

      this.selectedOption = this.matchingOptions[this.pointer];
      this.searchText = null;
      this.dropdownOpen = false;
      this.pointer = -1;
      this.$nextTick().then(() => {
        this.$refs.match.focus();
      });
      // EMIT HERE
      console.log('emitting value: ', this.selectedOption)
      this.$emit('selectValue', {
        index: this.index, 
        field: this.name,
        value: this.selectedOption});
    },
    handleClickOutside(e) {
      let box = this.$el.getBoundingClientRect();
      let clickX = e.clientX;
      let clickY = e.clientY;

      // is it inside?
      if (box.left < clickX && clickX < box.right && box.top < clickY && clickY < box.bottom){
        return;
      }
      this.dropdownOpen = false;
      this.searchText = null;
    },
    handleKeyOutside(e) {
      // this applies to all dropdowns in the page
      if (e.key === 'Tab' || e.key === 'Escape'){
        this.dropdownOpen = false;
        this.searchText = null;
      }
      // The following only to this specific instance
      if (!this.inFocus){return;}
      if (e.key === 'ArrowDown' && this.searchText === null){
        console.log('here')
        this.searchText = '';
        this.dropdownOpen = true;
      }
    },
    getStyle() {
      let style = 'height:' + this.componentHeight + ';'
      // style += 'color: ' + this.mainColor + ';'
      style += 'background-color: ' + this.backgroundColor + ';'
      return style;
    }
  }
};
</script>
  

<style scoped>
  .search-input {
    display: block;
    width: 100%;
    padding: 0.375em 0.75em;
    font-size: 1em;
    line-height: 1.5;
    color: #495057;
    /* background-color: #fff; */
    background-clip: padding-box;
    border: 1px solid #ced4da;
    border-radius: 0.25em;
    transition: border-color 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
    box-sizing: border-box;
  }
  .search-input:hover {
    border-color: black;
  }
  .icons {
    padding: 0 1em;
    right: 0;
    top: 0;
    bottom: 0;
    fill: #606f7b;
  }
  .icons svg {
    width: 0.75em;
    height: 0.75em;
  }
  .single-select-wrapper {
    position: relative;
    margin-bottom: 0.5em;
  }
  .wrapper-child {
    position: relative;
    display: inline-block;
    width: 100%;
  }
  .dropdown {
    -webkit-box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.12),
      0 2px 4px 0 rgba(0, 0, 0, 0.08);
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.12), 0 2px 4px 0 rgba(0, 0, 0, 0.08);
    background-color: #fff;
    color: #606f7b;
    border-radius: 0.25em;
    line-height: 1.25;
    text-align: left;
    position: absolute;
    width: 100%;
    overflow: auto;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    margin-top: 1px;
    list-style: none;
    padding: 0;
  }
  .dropdown > li {
    padding: 0.5em 0.75em;
    outline: 0;
    cursor: pointer;
  }
  .active {
    background: #dae1e7;
  }
  .icon-close  {
    display: flex;
    position: absolute;
    align-items: center;
  }
  .icon-arrow {
    position: absolute;
    cursor: pointer;
    align-items: center;
    display: flex;
    transition: all 0.5s;
  }
</style>