<template>
  <div
    ref="root"
    class="navbar-item has-dropdown has-dropdown-with-icons"
    :class="{ 'is-hoverable':isHoverable, 'is-active':isDropdownActive }"
    @click="toggle"
  >
    <a class="navbar-link is-arrowless">
      <slot />

      <mdicon name="chevron-down" />
    </a>
    <slot name="dropdown" />
  </div>
</template>

<script>
import { computed, ref, onMounted, onBeforeUnmount } from 'vue'

export default {
  name: 'NavBarMenu',
  props: {
    isHoverable: {
      type: Boolean,
      default: false
    }
  },
  setup (props) {
    const root = ref(null)

    const isDropdownActive = ref(false)

    const dropdownIcon = computed(
      () => isDropdownActive.value ? 'chevron-up' : 'chevron-down'
    )

    const toggle = () => {
      if (!props.isHoverable) {
        isDropdownActive.value = !isDropdownActive.value
      }
    }

    const forceClose = e => {
      if (!root.value.contains(e.target)) {
        isDropdownActive.value = false
      }
    }

    onMounted(() => {
      window.addEventListener('click', forceClose)
    })

    onBeforeUnmount(() => {
      window.removeEventListener('click', forceClose)
    })

    return {
      root,
      isDropdownActive,
      dropdownIcon,
      toggle
    }
  }
}
</script>
