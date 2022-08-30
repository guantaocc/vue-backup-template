
<script>
export default {
  name: 'Ellipsis',
  props: {
    content: {
      type: String,
      default: ''
    },
    direction: {
      type: String,
      validator: (value) => ['start', 'end', 'middle'].includes(value),
      default: 'start'
    },
    rows: {
      type: Number,
      default: 1
    },
    expandText: {
      type: String,
      default: ''
    },
    collapseText: {
      type: String,
      default: ''
    },
    stopPropagationForActionButtons: {
      type: Array,
      default: () => []
    },
    onConentClick: {
      type: Function,
      default: () => {}
    }
  },
  data() {
    return {
      expanded: false,
      exceeded: false,
      ellipsised: {
        leading: '',
        tailing: ''
      }
    }
  },
  render() {
    function pxToNumber(value) {
      if (!value) return 0
      const match = value.match(/^\d*(\.\d*)?/)
      return match ? Number(match[0]) : 0
    }
    // 计算文字是否需要截断
    const calcEllipsised = () => {
      const root = this.$refs.rootRef
      if (!root) return
      if (!root.offsetParent) return
      // 创建一个节点进行计算
      const container = document.createElement('div')
      const originStyle = window.getComputedStyle(root)
      const styleNames = Array.prototype.slice.apply(originStyle)
      styleNames.forEach(name => {
        container.style.setProperty(name, originStyle.getPropertyValue(name))
      })
      const lineHeight = pxToNumber(originStyle.lineHeight)
      const maxHeight = Math.floor(
        lineHeight * (this.rows + 0.5) +
        pxToNumber(originStyle.paddingTop) +
        pxToNumber(originStyle.paddingBottom)
      )
      container.style.position = 'fixed'
      container.style.left = '999999px'
      container.style.top = '999999px'
      container.style.zIndex = '-1000'
      container.style.height = 'auto'
      container.style.minHeight = 'auto'
      container.style.maxHeight = 'auto'
      container.style.textOverflow = 'clip'
      container.style.whiteSpace = 'normal'
      container.style.webkitLineClamp = 'unset'
      container.style.display = 'block'
      container.innerText = this.content
      document.body.appendChild(container)

      // 计算行是否超出
      if (container.offsetHeight <= maxHeight) {
        this.exceeded = false
      } else {
        // 超出截断文本
        this.exceeded = true
        const end = this.content.length
        const actionText = this.expanded ? '展开' : '收起'
        // 检查文本
        const check = (left, right) => {
          // 文本最小
          if (right - left <= 1) {
            // 单一文本
            if (this.direction === 'end') {
              return {
                leading: this.content.slice(0, left) + '...'
              }
            } else {
              return {
                tailing: '...' + this.content.slice(right, end)
              }
            }
          }
          const middle = Math.round((left + right) / 2)
          if (this.direction === 'end') {
            container.innerText =
            this.content.slice(0, middle) + '...' + actionText
          } else {
            container.innerText =
            actionText + '...' + this.content.slice(middle, end)
          }
          if (container.offsetHeight <= maxHeight) {
            if (this.direction === 'end') {
              return check(middle, right)
            } else {
              return check(left, middle)
            }
          } else {
            if (this.direction === 'end') {
              return check(left, middle)
            } else {
              return check(middle, right)
            }
          }
        }
        const checkMiddle = (leftPart,
          rightPart) => {
          if (
            leftPart[1] - leftPart[0] <= 1 &&
          rightPart[1] - rightPart[0] <= 1
          ) {
            return {
              leading: this.content.slice(0, leftPart[0]) + '...',
              tailing: '...' + this.content.slice(rightPart[1], end)
            }
          }
          const leftPartMiddle = Math.floor((leftPart[0] + leftPart[1]) / 2)
          const rightPartMiddle = Math.ceil((rightPart[0] + rightPart[1]) / 2)
          container.innerText =
          this.content.slice(0, leftPartMiddle) +
          '...' +
          actionText +
          '...' +
          this.content.slice(rightPartMiddle, end)
          if (container.offsetHeight <= maxHeight) {
            return checkMiddle(
              [leftPartMiddle, leftPart[1]],
              [rightPart[0], rightPartMiddle]
            )
          } else {
            return checkMiddle(
              [leftPart[0], leftPartMiddle],
              [rightPartMiddle, rightPart[1]]
            )
          }
        }
        const middle = Math.floor((0 + end) / 2)
        const ellipsised = this.direction === 'middle' ? checkMiddle([0, middle], [middle, end]) : check(0, end)
        this.ellipsised = ellipsised
        document.body.removeChild(container)
      }
    }
    this.$nextTick(() => {
      calcEllipsised()
    })
    const expandActionElement =
    this.expanded && this.expandText
      ? <a
        onClick={() => {
          this.expanded = true
        }}
      >
        {this.expandText}
      </a>
      : null
    const collapseActionElement =
    this.exceeded && this.expandText
      ? <a
        onClick={() => {
          this.expanded = false
        }}
      >
        {this.collapseText}
      </a>
      : null
    const renderContent = () => {
      if (!this.exceeded) {
        return this.content
      }
      if (this.expanded) {
        return (
          <span>
            {this.content}
            {collapseActionElement}
          </span>
        )
      } else {
        return (
          <span>
            {this.ellipsised.leading}
            {expandActionElement}
            {this.ellipsised.tailing}
          </span>
        )
      }
    }
    return <div ref='rootRef' class='one-ellipsise'>{renderContent()}</div>
  }
}
</script>

<style lang="scss" scoped>

</style>
