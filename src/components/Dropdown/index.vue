<script setup name="Dropdown">
const props = defineProps({
  option: {
    type: Object,
    default: () => ({})
  },
  isEditColor: {
    type: Boolean,
    default: false
  },
  trigger: {
    type: String,
    default: 'click'
  },
  isArrowIcon: {
    type: Boolean,
    default: false
  },
  width: {
    type: Number, 
    default: 75
  }
})
const state = reactive({
  rotateArrow: 0,
  translateValue: 0,
  dropdownHeight: 0,
  listOpacity: 0
})
const width = computed(() => `${props.width}px`)
const setDropdownProps = (deg, ht, opacity) => {
  state.rotateArrow = deg !== 0 ? deg + 'deg' : 0
  state.dropdownHeight = ht !== 0 ? ht + 'rem' : 0
  state.listOpacity = opacity
}
const colorModel = defineModel('color')
const emit = defineEmits(['command'])

const handleCommand = (data) => {
  console.log(data)
  setDropdownProps(0, 0, 0)
  emit('command', data)
}
const onListMouseover = (index) => {
  state.translateValue = `${index * 2}rem`
}
const dropdownHidden = () => {
  const list_wrapper_sizes = 3.5
  const dropdown_open_height =
    2 * props.option.length + list_wrapper_sizes + (props.isEditColor ? 2 : 0)
  const curr_dropdown_height = state.dropdownHeight || 0
  curr_dropdown_height === 0
    ? setDropdownProps(180, dropdown_open_height, 1)
    : setDropdownProps(0, 0, 0)
}
</script>
0
<template>
  <div class="dropdown-container">
    <button
      class="dropdown-button main-button"
      @mouseover="props.trigger === 'hover' ? dropdownHidden() : null"
      @click="props.trigger === 'click' ? dropdownHidden() : null"
    >
      <slot />
      <span class="dropdown-arrow" v-if="props.isArrowIcon">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16">
          <path
            d="M7.247 11.14 2.451 5.658C1.885 5.013 2.345 4 3.204 4h9.592a1 1 0 0 1 .753 1.659l-4.796 5.48a1 1 0 0 1-1.506 0z"
          />
        </svg>
      </span>
    </button>
    <div class="dropdown-list-container" @mouseleave="setDropdownProps(0, 0, 0)">
      <div class="dropdown-list-wrapper">
        <ul class="dropdown-list">
          <li
            class="dropdown-list-item"
            v-for="(item, index) in props.option"
            @mouseover="onListMouseover(index)"
            :key="index"
          >
          <button class="dropdown-button list-button" @click="handleCommand(item)">
            <slot neme="dropdown" :item="item"></slot>
            </button>
          </li>
          <li
            class="dropdown-list-item"
            @mouseover="onListMouseover(props.option.length)"
            :key="props.option.length"
            v-if="props.isEditColor"
          >
            <button class="dropdown-button list-button">
              <input type="color" v-model="colorModel" />
            </button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<style scoped>
button {
  border: none;
  background-color: transparent;
  cursor: pointer;
  outline: none;
}
svg {
  width: 1rem;
  height: 1rem;
}
.text-truncate {
  /* 超出显示省略号 */
  text-overflow: ellipsis;
  overflow: hidden;
  /* 不换行 */
  white-space: nowrap;
}
.dropdown-container {
  display: flex;
  flex-direction: column;
}
.dropdown-title-icon,
.dropdown-arrow {
  display: inline-flex;
}
.dropdown-title {
  margin: 0 auto 0 1.8rem;
}
.dropdown-button {
  font-weight: 400;
  display: flex;
  justify-content: space-around;
  align-items: center;
  padding-left: 10px;
  padding-right: 10px;
}
.dropdown-button svg {
  /* 填充颜色 */
  fill: #b1b8ca;
  /* 设置过渡: 全部 时长 贝塞尔曲线 */
  transition: all 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}
.dropdown-button svg,
.dropdown-button span {
  /* 元素不对指针事件做出反应 */
  pointer-events: none;
}
.dropdown-button:hover,
.dropdown-button:focus {
  color: #fff;
}
.dropdown-button:hover svg,
.dropdown-button:focus svg {
  fill: #fff;
}
.main-button {
  height: 2rem;
  width: v-bind(width);
  background-color: #333740;
  color: #b1b8ca;
  border-radius: 5px;
  border: 0.1rem solid #494d59;
  transition: all 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}
.main-button:focus {
  border: 0.1rem solid #2c62f6;
  box-shadow: 0 0 0 0.2rem rgba(44, 98, 246, 0.4);
}
.main-button .dropdown-arrow {
  margin-left: 1rem;
  /* 通过var函数调用自定义属性,设置旋转角度 */
  transform: rotate(v-bind('state.rotateArrow'));
  transition: transform 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}
.list-button {
  height: 2rem;
  color: #b1b8ca;
  /* 溢出隐藏 */
  overflow: hidden;
  transition: color 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}
.list-color {
  height: 2rem;
  color: #b1b8ca;
  /* 溢出隐藏 */
  overflow: hidden;
  transition: color 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}
.dropdown-list-container {
  overflow: hidden;
  position: absolute;
  margin-top: 2.2rem;
  max-height: v-bind('state.dropdownHeight');
  transition: max-height 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  z-index: 1;
}
.dropdown-list-wrapper {
  background-color: #333740;
  border-radius: 5px;
  border: 0.1rem solid #494d59;
  position: relative;
}
ul.dropdown-list {
  position: relative;
  list-style-type: none;
}
ul.dropdown-list::before {
  content: '';
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
  z-index: 0;
  opacity: 0;
  height: 2rem;
  background-color: #606266;
  border-radius: 5px;
  pointer-events: none;
  transform: translateY(v-bind('state.translateValue'));
  transition: all 0.2s linear;
}
li.dropdown-list-item {
  display: flex;
  flex-direction: column;
  position: relative;
  z-index: 1;
  opacity: v-bind('state.listOpacity');
  transition: opacity 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}
ul.dropdown-list:hover::before,
ul.dropdown-list:hover {
  opacity: 1;
}
</style>
