<template>
  <div class="lb-drag-and-drop">
    <slot @hook:mounted="slotMounted"></slot>
  </div>
</template>
<script>
// 创建shaodw 元素
const shadowElem = document.createElement('div')
shadowElem.className = 'shadow'
shadowElem.style.cssText = `

`

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
      this.initChildEvent()
    },
    initChildEvent() {
      const self = this
      this.childElem.forEach(child => {
        child.cssList = `
          transition: all 500ms;
        `
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
          console.log(shadowElem)
          elem.parentElement.replaceChild(shadowElem, elem)
          // 设置样式
          elem.style.cssText = `
            transform: scale(1.2);
            box-shadow: 0 3px 15px #ddd;
            position: fixed;
            left: ${left};
            top: ${top}
          `
          // 设定开始锚点
        })
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
<style lang='stylus' scoped>
</style>