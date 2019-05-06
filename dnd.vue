<template>
  <div class="lb-drag-and-drop">
    <slot @hook:mounted="slotMounted"></slot>
  </div>
</template>
<script>
// 创建shaodw 元素
let shadowElem = document.createElement('div')
shadowElem.className = 'dnd-shadow'
let activeElem = null

let bacupCssText = ''

/**
 * 交换两个元素位置
 */
function exchangeElement(a, b) {
  const { left: aLeft, top: aTop } = a.getBoundingClientRect()
  const { left: bLeft, top: bTop } = b.getBoundingClientRect()
  // init style
  a.style.position = b.style.position = 'relative'
  a.style.transition = b.style.transition = 'all 200ms'
  a.style.top = b.style.top = '0px'
  a.style.left = b.style.left = '0px'
  // transition
  setTimeout(() => {
    a.style.left = bLeft - aLeft + 'px'
    a.style.top = bTop - aTop + 'px'
    b.style.left = aLeft - bLeft + 'px'
    b.style.top = aTop - bTop + 'px'
  }, 0);
  // replace element

}
window._e = exchangeElement
export default {
  name: 'lb-dnd',
  data() {
    return {
      childElem: [],
      childElemPos: [],
      mousedown: false,
      offsetX: 0,
      offsetY: 0
    };
  },
  components: {

  },
  computed: {

  },
  mounted() {
    this.init(
    )
  },
  methods: {
    init() {
      this.childElem = this.$slots.default.map(s => s.elm)
      this.childElem.forEach(item => {
        this.initChildClass(item)
        this.childElemPos.push(item.getBoundingClientRect())
      }, this)
      this.initEvent()
    },
    initChildClass(child) {
      child.classList.add('dnd-item')
    },
    initEvent(child) {
      document.addEventListener('mousedown', e => {
        const elem = e.target
        if (!elem.classList.contains('dnd-item')) return
        activeElem = elem
        this.mousedown = true
        // 获取尺寸
        const { width, height, top, left } = elem.getBoundingClientRect()
        const { display, position, margin } = getComputedStyle(elem)
        // 将shadow 元素 替换到 原位置处
        shadowElem.style.cssText += `
          display: ${display};
          position: ${position};
          margin: ${margin};
          width: ${width}px;
          height: ${height}px;
          box-sizing: border-box;
          border: 2px dashed #ddd;
          background: #eee;
        `
        // 设置样式
        elem.classList.add('dnd-item-active')
        elem.style.left = `${left}px`
        elem.style.top = `${top}px`


        this.$el.insertBefore(shadowElem, elem)
        // 获取鼠标下落时和元素的 offset
        this.offsetX = parseInt(e.clientX) - left
        this.offsetY = parseInt(e.clientY) - top
        // 绑定move 事件
        document.addEventListener('mousemove', this.handleMove)
      })

      document.addEventListener('mouseup', e => {
        this.mousedown = false
        // 放回去,替换掉shadow元素
        this.$el.insertBefore(activeElem, shadowElem)
        shadowElem.remove()
        // 缩放回去大小
        activeElem.classList.remove('dnd-item-active')
        activeElem.style.cssText = ''
        document.removeEventListener('mousemove', this.handleMove)
      })
    },
    handleMove(e) {
      // 移动元素
      activeElem.style.left = `${parseInt(e.clientX) - this.offsetX}px`
      activeElem.style.top = `${parseInt(e.clientY) - this.offsetY}px`
      // 监测 & 更新 shadow元素位置
      this.childElemPos.forEach(pos => {

      })
    },
    hookMounted() {
      console.log('hook mounted')
    }
  },
  watch: {

  }
};
</script>
<style scoped>
  .dnd-item{
    transition: transform 500ms;
    transform-origin: center;
  }
  .dnd-item-active{
    background: #fff;
    transform: scale(1.1);
    box-shadow: 0 3px 15px #ddd;
    position: fixed;
    margin: 0 !important;
    opacity: 0.9;
    cursor: pointer;
    user-select: none
  }
  /* .dnd-shadow{
    box-sizing: border-box;
    border: 1px dashed;
    background: lightcoral;
  } */
</style>