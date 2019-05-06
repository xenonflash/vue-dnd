<template>
  <div class="lb-drag-and-drop">
    <slot @hook:mounted="slotMounted"></slot>
  </div>
</template>
<script>
// 创建shaodw 元素
const shadowElem = document.createElement('div')
shadowElem.className = 'shadow'
const activeElem = null

let bacupCssText = ''

export default {
  name: 'lb-dnd',
  data() {
    return {
      childElem: []
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
        this.initChildEvent(item)
      }, this)
    },
    initChildClass(child) {
      child.classList.add('dnd-item')
    },
    initChildEvent(child) {
      child.addEventListener('mousedown', e => {
        const elem = e.target
        // 获取尺寸
        const { width, height, top, left } = elem.getBoundingClientRect()
        const { display, position, margin } = getComputedStyle(elem)
        // 将shadow 元素 替换到 原位置处
        shadowElem.style.cssText = `
          display: ${display};
          position: ${position};
          margin: ${margin};
          width: ${width}px;
          height: ${height}px;
          box-sizing: border-box;
          border: 1px dashed;
          background: lightred;
        `
        // 设置样式
        elem.classList.add('dnd-item-active')
        elem.style.cssText += `
          left: ${left}px;
          top: ${top}px
        `
        this.$el.insertBefore(shadowElem, elem)
        // 设定开始锚点
      })

      child.addEventListener('mouseup', e => {
        const elem = e.target
        // 放回去,替换掉shadow元素
        this.$el.replaceChild(elem, shadowElem)
        // 缩放回去大小
        elem.classList.remove('dnd-item-active')
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
  }
</style>