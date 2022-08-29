
<script>
export default {
  name: 'FormField',
  render(h) {
    const { component, props } = this.$attrs
    const componentName = `el-${component}`

    // 如果是 选项
    const children = []
    if (component === 'select') {
      const { options } = props
      if (!options) {
        return new Error('select component must have options')
      }
      const childNode = options.map(item => {
        return <el-option key={item.key} label={item.label} value={item.value}></el-option>
      })
      children.push(childNode)
    }
    return h(componentName, {
      props: { ...this.$attrs, ...props },
      on: { 'input': (value) => this.$emit('input', value), ...this.$listeners }
    }, children)
  }
}
</script>

<style lang="scss" scoped>

</style>
