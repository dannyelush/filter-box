<!-- eslint-disable vue/no-v-html -->
<template>
  <div class="filter-box-container">
    <div class="panel panel-default">
      <div class="panel-heading">
        <a
          ref="panelcollapse"
          :href="'#panel-expanded-' + dropId"
          :class="{
            collapsed: toggled,
          }"
          role="button"
          data-toggle="panel-collapse"
          class="panel-toggle"
          >
          {{ panelHeader }}
          <span
            class="toggle-sign"
            v-html="toggled ? escapedCollapsedIcon : escapedCollapseIcon"
            />
        </a>
      </div>

      <div
        :id="'panel-expanded-' + dropId"
        ref="panelbody"
        class="panel-collapse collapsed"
        >
        <div class="panel-body">
          <div>
            <div
              v-if="search && options.length > 0"
              class="search-box"
              >
              <input
                v-model="filter"
                class="form-control"
                :placeholder="mergedTexts.searchText"
                aria-label="search"
                >
            </div>

            <p
              v-if="options.length === 0"
              class="no-options-text"
              >
              {{ mergedTexts.placeholder }}
            </p>

            <span
              v-if="emptyResults && options.length > 0"
              class="text-muted empty"
              >
              {{ mergedTexts.empty }}
            </span>

            <ul
              v-else
              class="dropdown-menu items"
              >
              <li
                v-for="option in filtered.sort(sortSelectedFirstFunction)
                  .slice(0, Math.min(linearOptions.length, displayMax))"
                :key="option.value || option.header"
                :data-value="option.value"
                class="simple-option"
                :class="{
                  'dropdown-header': option.header,
                  active: !multiple && !!selected[option.value],
                  disabled: option.disabled,
                  'has-header': !!option.parentHeader,
                  multiple: multiple,
                }"
                >
                <span
                  class="selectable-option"
                  >
                  <label
                    :for="dropId + '-' + option.value"
                    class="simple-label"
                    :class="{ disabled: option.disabled }"
                    >
                    <input
                      :id="dropId + '-' + option.value"
                      :type="multiple ? 'checkbox' : 'radio'"
                      :disabled="option.disabled"
                      :checked="multiple ? modelValue.indexOf(option.value) > -1
                        : modelValue === option.value"
                      @change="select($event, option.value)"
                      @click="click($event, option.value)"
                      >
                    <span
                      v-if="option.icon"
                      class="icon-container"
                      v-html="escapeTextSafe(option.icon)"
                      />{{ option.text }}
                    <span class="pull-right pr-0">
                      <span
                        v-if="option.count !== undefined"
                        class="text-note p-0"
                        >({{ option.count }})</span>
                      <span
                        v-if="!multiple && modelValue === option.value"
                        class="checked"
                        >
                        &#10004;
                      </span>
                    </span>
                  </label>
                </span>
              </li>
            </ul>
            <a
              v-if="linearOptions.length > displayMax"
              href="#"
              class="showMoreLink"
              @click.prevent.stop="togglePopup"
              >{{ mergedTexts.showMore }} ({{ linearOptions.length }})</a>
          </div>
        </div>
      </div>
    </div>

    <div
      v-if="linearOptions.length > displayMax"
      class="big-popup"
      :class="{ open: isOpen }"
      >
      <div class="popup-header">
        {{ panelHeader }}
        <button
          type="button"
          class="close"
          aria-label="Close"
          @click="closePopup"
          >
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="popup-body">
        <div
          v-if="search"
          class="popup-search-box"
          >
          <input
            v-model="filter"
            class="form-control"
            :placeholder="mergedTexts.searchText"
            aria-label="search"
            >
        </div>
        <span
          v-if="emptyResults"
          class="text-muted empty"
          >
          {{ mergedTexts.empty }}
        </span>
        <div
          v-else
          class="letters-container"
          >
          <div
            v-for="key in Object.keys(groupedOptions)"
            :key="key"
            >
            <h3 class="group-heading">
              {{ key }}
            </h3>
            <div class="letter-elements">
              <div
                v-for="item in groupedOptions[key]"
                :key="item.value"
                class="letter-item simple-option"
                :class="{
                  active: !multiple && popupSelected && popupSelected.indexOf(item.value) > -1,
                }"
                >
                <label
                  :for="dropId + '-popup-item-' + item.value"
                  class="simple-label"
                  :class="{ disabled: item.disabled }"
                  >
                  <input
                    :id="dropId + '-popup-item-' + item.value"
                    :type="multiple ? 'checkbox' : 'radio'"
                    :disabled="item.disabled"
                    :checked="multiple ? popupSelected.indexOf(item.value) > -1
                      : popupSelected === item.value"
                    @click="click($event, item.value)"
                    @change="selectFromPopup($event, item.value)"
                    >
                  <span
                    v-if="item.icon"
                    class="icon-container"
                    v-html="escapeTextSafe(item.icon)"
                    />{{ item.text }}
                  <span
                    v-if="item.count !== undefined"
                    class="text-note"
                    >({{ item.count }})</span>
                  <span
                    v-if="!multiple && popupSelected === item.value"
                    class="checked pull-right"
                    >
                    &#10004;
                  </span>
                </label>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="popup-footer text-right">
        <button
          type="button"
          class="btn btn-link"
          @click="removeFilters"
          >
          Remove filters
        </button>
        <button
          type="button"
          class="btn btn-primary"
          @click="applyFilters"
          >
          Apply filters
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch, nextTick, onMounted } from 'vue';

const props = defineProps({
  modelValue: {
    default: null,
    type: [String, Array],
  },
  multiple: {
    default: true,
    type: Boolean,
  },
  search: {
    default: false,
    type: Boolean,
  },
  controls: {
    default: false,
    type: Boolean,
  },
  collapseHeaders: {
    default: false,
    type: Boolean,
  },
  displayMax: {
    default: 20,
    type: Number,
  },
  dropdownClass: {
    default: '',
    type: String,
  },
  options: {
    default() {
      return [];
    },
    type: Array,
  },
  texts: {
    default: () => ({}),
    type: Object,
  },
  panelHeader: {
    default: '',
    type: String,
  },
  dropId: {
    default: 'key',
    type: String,
  },
  isExpanded: {
    default: false,
    type: Boolean,
  },
  sortSelectedFirst: {
    default: false,
    type: Boolean,
  },
});

const emit = defineEmits(['update:modelValue', 'filter']);

function getDefaultValue() {
  if (props.modelValue !== null) {
    return props.modelValue;
  }
  if (props.multiple) {
    return [];
  }
  return null;
}

const myValue = ref(getDefaultValue());
const filter = ref('');
const isOpen = ref(false);
const popupSelected = ref([]);
const toggled = ref(false);

const mergedTexts = computed(() => ({
  searchText: 'Search',
  empty: 'No results',
  showMore: 'Show more',
  collapsedIcon: '-',
  collapseIcon: '+',
  placeholder: 'No options to display',
  ...props.texts,
}));

const safeTags = ['b', 'i', 'em', 'strong'];
function escapeHtml(str) {
  const div = document.createElement('div');
  div.appendChild(document.createTextNode(str));
  return div.innerHTML;
}

function escapeTextSafe(nonEscapedText) {
  let text = escapeHtml(nonEscapedText);
  text = text.replace(/&lt;br(\s\/)?&gt;/ig, '<br>');
  safeTags.forEach((t) => {
    text = text.replace(new RegExp(`&lt;${t}( class=".*")?&gt;`, 'ig'), `<${t}$1>`);
    text = text.replace(new RegExp(`&lt;/${t}&gt;`, 'ig'), `</${t}>`);
  });
  return text;
}

const escapedCollapsedIcon = computed(() => escapeTextSafe(mergedTexts.value.collapsedIcon));

const escapedCollapseIcon = computed(() => escapeTextSafe(mergedTexts.value.collapseIcon));

function getOptionsMap(options, map = {}) {
  return options.value.reduce((m, o) => {
    if (!o.header) {
      Object.assign(m, { [o.value]: o });
    }
    return m;
  }, map);
}

const linearOptions = computed(() => props.options.reduce((f, o) => {
  if (o.options) {
    // push the header
    f.push({
      header: o.label,
    });
    f.push(...o.options.map((opt) => Object.assign(opt, { parentHeader: o.label })));
  } else {
    // it's an item without group, push it to the list
    f.push(o);
  }
  return f;
}, []));

// For the optionsMap, use all options, not just the filtered ones
// so that selecting values searches entire list
const optionsMap = computed(() => getOptionsMap(linearOptions));

const selected = computed(() => {
  let selectedValue = {};
  if (myValue.value != null) {
    let theVal = myValue.value;
    if (!props.multiple) {
      theVal = [myValue.value];
    }
    selectedValue = theVal.reduce((s, v) => {
      if (optionsMap.value[v]) {
        Object.assign(s, {
          [v]: optionsMap.value[v],
        });
      }
      return s;
    }, {});
  }
  return selectedValue;
});

const filterRegExp = computed(() => new RegExp(`${filter.value}`, 'ig'));

function textMatch(text) {
  return text ? text.match(filterRegExp.value) !== null : true;
}

function optionMatch(o) {
  const textMatches = textMatch(o.text || o.header);
  return textMatches;
}
const filtered = computed(() => linearOptions.value.filter(optionMatch));

const groupedOptions = computed(() => {
  const groups = filtered.value.reduce((r, e) => {
    const group = e.text ? e.text[0].toLowerCase() : '*';
    if (!r[group]) {
      // eslint-disable-next-line no-param-reassign
      r[group] = [e];
    } else {
      r[group].push(e);
    }
    return r;
  }, {});
  const sortedGroups = {};
  Object.keys(groups).sort().forEach((g) => {
    sortedGroups[g.toUpperCase()] = groups[g];
  });
  return sortedGroups;
});

const emptyResults = computed(() => props.search && filtered.value.length === 0 && filter);

watch(myValue, (newMyValue) => {
  emit('update:modelValue', newMyValue);
});

watch(() => props.modelValue, (newModelValue) => {
  myValue.value = newModelValue;
});

const panelbody = ref(null);

function validateOptions(options, l = 0) {
  options.forEach((o, i) => {
    if (o.options) {
      if (!o.label) {
        console.warn(`No label specified for entry at position ${i}, level ${l}`);
      }
      validateOptions(o.options, l + 1);
    } else if (!o.text) {
      console.warn(`No text specified for entry at position ${i}, level ${l}`);
    }
  });
}

const sorter = new Intl.Collator('en', { sensitivity: 'base', numeric: true });

function sortSelectedFirstFunction(a, b) {
  if (!props.sortSelectedFirst) {
    return 0;
  }
  const aIsSelected = (props.modelValue.indexOf(a.value) === -1) + a.text;
  const bIsSelected = (props.modelValue.indexOf(b.value) === -1) + b.text;
  return sorter.compare(aIsSelected, bIsSelected);
}

watch(() => props.options, async () => {
  validateOptions(props.options);

  await nextTick();
  const content = panelbody.value;
  if (props.isExpanded && content) {
    content.style.maxHeight = `${Math.max(content.scrollHeight, 250)}px`;
  }
}, { immediate: true, deep: true });

watch(() => props.isExpanded, (newIsExpanded) => {
  toggled.value = !newIsExpanded;
}, { immediate: true });

watch(filter, (newFilter) => {
  emit('filter', newFilter);
});

const panelcollapse = ref(null);

function closePopup() {
  isOpen.value = false;
  filter.value = '';
  popupSelected.value = props.multiple ? [...myValue.value] : myValue.value;
}

onMounted(() => {
  document.addEventListener('click', (event) => {
    // If user clicks inside the element, do nothing
    if (event.target.closest('.filter-box-container')) return;
    // If user clicks outside the element, hide it!
    closePopup();
  });

  // Init panels
  const panel = panelcollapse;
  const content = panelbody;
  panel.value.addEventListener('click', (e) => {
    e.preventDefault();
    if (content.value.style.maxHeight) {
      content.value.style.maxHeight = null;
      toggled.value = true;
    } else {
      content.value.style.maxHeight = `${Math.max(content.value.scrollHeight, 250)}px`;
      toggled.value = false;
    }
  });
});

function click(e) {
  if (!props.multiple) {
    if (e.target.checked) {
      popupSelected.value = '';
      myValue.value = '';
    }
  }
}

function select(e, val) {
  e.preventDefault();
  if (optionsMap.value[val].disabled) {
    e.stopPropagation();
    return;
  }
  let newVal;
  // for multiple, don't close the menu
  if (props.multiple) {
    e.stopPropagation();
    // update the new selected items
    newVal = Object.keys(selected.value).map((k) => selected.value[k].value);
    if (selected.value[val]) {
      // remove it
      newVal = newVal.filter((k) => k !== val);
    } else {
      // add it
      newVal.push(val);
    }
  } else {
    // for single mode, just set it directly
    newVal = val;
  }
  popupSelected.value = props.multiple ? [...newVal] : newVal;
  myValue.value = newVal;
}

function selectFromPopup(e, val) {
  e.preventDefault();
  if (optionsMap.value[val].disabled) {
    e.stopPropagation();
    return;
  }
  // for multiple, don't close the menu
  if (props.multiple) {
    e.stopPropagation();
    // update the new selected items
    if (popupSelected.value.indexOf(val) === -1) {
      // add it
      popupSelected.value.push(val);
    } else {
      // remove it
      popupSelected.value = popupSelected.value.filter((k) => k !== val);
    }
  } else {
    // for single mode, just set it directly
    popupSelected.value = val;
  }
}

function selectNone() {
  if (props.multiple) {
    myValue.value = [];
  } else {
    myValue.value = null;
  }
}
function togglePopup() {
  isOpen.value = !isOpen.value;
  filter.value = '';
}

function removeFilters() {
  selectNone();
  closePopup();
}
function applyFilters() {
  myValue.value = props.multiple ? [...popupSelected.value] : popupSelected.value;
  closePopup();
}
</script>

<style scoped>
input[type="checkbox"],
input[type="radio"] {
  margin-top: 0;
  margin-right: 5px;
}
.filter-box-container {
  position: relative;
}
.simple-label {
  font-weight: normal;
  width: 100%;
  margin-bottom: 0;
  cursor: pointer;
  font-size: 14px;
  line-height: 26px;
  display: block;
}

.simple-label input[type='radio'] {
  display: none;
}

.simple-label.disabled {
  color: #C4CDD5;
  cursor: default;
}

.simple-label.disabled .text-note {
  color: #C4CDD5;
}

.simple-option {
  padding-left: 10px;
  padding-right: 10px;
  border-left: 3px solid transparent;
}
.simple-option.multiple {
  padding-left: 0;
}
.simple-option:hover {
  background-color: #E8F8FF;
}

.simple-option.active {
  background-color: #E8F8FF;
  border-left: 3px solid #019FE1;
}

.selectable-option span {
  display: inline;
}
.pull-right {
  float: right;
}
.text-right {
  text-align: right;
}
.panel {
  border-top: 1px solid #ffffff;
  border-left: 1px solid #ffffff;
  box-shadow: 0 0 12px 2px #E1EDF2;
  color: #37474F;
  border-radius: 3px;
}
.panel .panel-heading {
  border-bottom: none;
  padding-top: 7px;
  padding-bottom: 5px;
  border-top-right-radius: 3px;
  border-top-left-radius: 3px;
  color: #263238;
  background-color: #ffffff;
  padding: 10px 20px;
}
.panel .panel-heading a {
  font-size: 16px;
  color: #516373;
}
.panel .panel-heading .panel-toggle {
  width: 100%;
  display: inline-block;
  text-decoration: none;
  text-align: left;
}
.panel .panel-heading .panel-toggle .toggle-sign {
  float: right;
  color: #516373;
}
.panel .panel-collapse {
  overflow: visible;
}
.panel .panel-collapse.collapsed {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.2s ease-out;
}
.panel-collapse .panel-body {
  border-bottom-right-radius: 3px;
  border-bottom-left-radius: 3px;
  padding: 10px 0 20px 20px;
}

.form-control {
  display: block;
  width: 100%;
  height: 34px;
  padding: 6px 10px;
  font-size: 14px;
  line-height: 1.42857143;
  color: #37474F;
  background-color: #fff;
  background-image: none;
  border: 1px solid #C4CDD5;
  border-radius: 3px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  -webkit-transition: border-color ease-in-out 0.15s, box-shadow ease-in-out 0.15s;
  -webkit-transition: border-color ease-in-out 0.15s, -webkit-box-shadow ease-in-out 0.15s;
  transition: border-color ease-in-out 0.15s, -webkit-box-shadow ease-in-out 0.15s;
  transition: border-color ease-in-out 0.15s, box-shadow ease-in-out 0.15s;
  transition: border-color ease-in-out 0.15s, box-shadow ease-in-out 0.15s,
        -webkit-box-shadow ease-in-out 0.15s;
  box-sizing: border-box;
}
.search-box {
  margin-bottom: 10px;
  padding-right: 20px;
}
.p-0 {
  padding: 0;
  line-height: normal;
}
.pr-0 {
  padding: 0;
  line-height: 26px;
}
.checked {
  font-weight: bolder;
  padding: 0;
}
.text-note {
  color: #516373;
  font-size: 12px;
}
.dropdown-menu {
  padding-right: 13px;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
}
.dropdown-menu > li {
  margin-bottom: 5px;
}
.dropdown-menu > li > span {
  padding-left: 0;
  padding-right: 0;
}
.showMoreLink {
  margin-top: 5px;
  display: block;
}
.big-popup.open {
  display: block;
}
.icon-container > :first-child {
  margin-right: 5px;
}
.popup-header {
  border-bottom: 1px solid #C4CDD5;
  padding: 10px 20px;
  font-size: 16px;
}
.popup-header button.close {
  float: right;
  font-size: 38px;
  font-weight: normal;
  line-height: 20px;
  color: #37474F;
  text-shadow: 0 1px 0 #fff;
  padding: 0;
  cursor: pointer;
  background: transparent;
  border: 0;
  -webkit-appearance: none;
}
.popup-body {
  padding: 10px 0 0 0;
}
.popup-body .popup-search-box {
  padding-right: 20px;
  padding-left: 20px;
  padding-bottom: 10px;
  border-bottom: 1px solid #C4CDD5;
}
.popup-body .popup-search-box input {
  width: 60%;
  min-width: 200px;
}

.popup-body .letters-container {
  max-height: 350px;
  overflow-y: auto;
  padding-left: 20px;
}
.popup-body .letters-container h3.group-heading {
  font-size: 18px;
  font-weight: 400;
  line-height: 20px;
  color: #263238;
}
.popup-body .letter-elements {
  display: flex;
  flex-flow: row wrap;
  text-align: left;
}
.popup-body  .letter-item {
  width: 32%;
  margin-right: 1%;
  margin-bottom: 10px;
}

.popup-body .letter-item:nth-child(3) {
  margin-right: 0;
}
.popup-footer {
  border-top: 1px solid #C4CDD5;
  padding: 10px 20px;
}
.btn + .btn {
  margin-left: 20px;
}

.big-popup {
  position: absolute;
  top: 0;
  left: 0;
  width: 720px;
  display: none;
  z-index: 1001;
  background-color: #FFFFFF;
  border: 1px solid #C4CDD5;
  border-radius: 3px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  box-shadow: 0 0 12px 2px #E1EDF2;
}
.dropdown-menu {
  margin-top: 20px;
  width: 100%;
}
.dropdown-menu > li > a {
  position: relative;
  padding-right: 26px;
  text-overflow: ellipsis;
  overflow: hidden;
}
.dropdown-menu > li > a > i {
  position: absolute;
  top: 6px;
  right: 6px;
}
.dropdown-menu > li > span {
  text-overflow: ellipsis;
  overflow: hidden;
  display: block;
  position: relative;
}
.dropdown-menu > li > span > a {
  position: absolute;
  right: 0;
}
.dropdown-menu > .dropdown-menu {
  min-width: 302px;
}
.empty {
  padding: 10px 0;
  width: 100%;
  float: left;
  text-align: center;
  width: 100%;
  color: #757575;
  font-size: 16px;
}
.big-popup .empty {
  padding: 20px;
  float:none;
  display: block;
}

.items {
  display: block;
  position: relative;
  padding: 0;
  margin: 0;
  border: 0;
  box-shadow: none;
  width: 100%;
  max-height: 155px;
  overflow-y: auto;
  overflow-x: hidden;
  background-color: transparent;
}
.items > li {
  margin-right: 2px;
}
</style>
