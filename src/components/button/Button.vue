<script lang="ts" setup>
import { computed, toRefs, withDefaults } from 'vue'

// 按钮类型
type buttonType = 'primary' | 'success' | 'warning' | 'danger' | 'info' | 'text'
type buttonSize = 'medium' | 'small' | 'mini'
type buttonNativeType = 'button' | 'submit' | 'reset'

interface ButtonProps {
  type?: buttonType
  size?: buttonSize
  round?: boolean
  plain?: boolean
  nativeType?: buttonNativeType
  circle?: boolean
  icon?: string
  loading?: boolean
  disabled?: boolean
}

// withDefaults 使用类型声明时的默认 props 值
const props = withDefaults(defineProps<ButtonProps>(), {
  nativeType: 'button',
})

function useClasses({ props }) {
  return computed(() => {
    return [
      props.type ? `el-button--${props.type}` : '',
      props.size ? `el-button--${props.size}` : '',
      {
        'is-round': props.round,
        'is-plain': props.plain,
        'is-circle': props.circle,
        'is-disabled': props.disabled,
      },
    ]
  })
}
const classes = useClasses({ props })
</script>

<template>
  <!-- props size  medium / small / mini -->
  <button
    class="el-button"
    :class="classes"
    :type="nativeType"
    :disabled="disabled || loading"
  >
    <i v-if="loading" class="el-icon-loading" data-testid="loadingIcon" />
    <i v-else-if="icon" :class="icon" data-testid="icon" />
    <span v-if="$slots.default">
      <slot />
    </span>
  </button>
</template>

<style lang="scss">
@import "../../theme/src/button.scss";
</style>
