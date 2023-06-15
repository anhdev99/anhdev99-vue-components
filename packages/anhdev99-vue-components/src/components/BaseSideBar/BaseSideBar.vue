<script>
import simplebar from 'simplebar-vue'

import SideNav from '../BaseSideNav/BaseSideNav.vue'

/**
 * Sidebar component
 */
export default {
  components: { simplebar, SideNav },
  props: {
    isCondensed: {
      type: Boolean,
      default: false
    },
    type: {
      type: String,
      required: false,
      default: 'vertical'
    },
    width: {
      type: String,
      required: false,
      default: 'fluid'
    },
    menuItems: {
      type: Array,
      required: false
    }
  },
  data() {
    return {
      settings: {
        minScrollbarLength: 60
      }
    }
  },
  computed: {},
  watch: {
    $route: {
      handler: 'onRoutechange',
      immediate: true,
      deep: true
    },
    type: {
      immediate: true,
      handler(newVal, oldVal) {
        if (newVal !== oldVal) {
          switch (newVal) {
            case 'dark':
              document.body.setAttribute('data-sidebar', 'dark')
              document.body.removeAttribute('data-topbar')
              document.body.removeAttribute('data-sidebar-size')
              document.body.classList.remove('vertical-collpsed')
              break
            case 'light':
              document.body.setAttribute('data-topbar', 'dark')
              document.body.removeAttribute('data-sidebar')
              document.body.removeAttribute('data-sidebar-size')
              document.body.classList.remove('vertical-collpsed')
              break
            case 'compact':
              document.body.setAttribute('data-sidebar-size', 'small')
              document.body.setAttribute('data-sidebar', 'dark')
              document.body.setAttribute('data-topbar', 'light')
              document.body.classList.remove('vertical-collpsed')
              document.body.removeAttribute('data-keep-enlarged')
              break
            case 'icon':
              document.body.classList.add('vertical-collpsed')
              document.body.setAttribute('data-keep-enlarged', 'true')
              document.body.removeAttribute('data-sidebar-size')
              break
            case 'colored':
              document.body.setAttribute('data-sidebar', 'colored')
              document.body.removeAttribute('data-keep-enlarged')
              document.body.classList.remove('vertical-collpsed')
              break
            default:
              document.body.setAttribute('data-sidebar', 'dark')
              break
          }
        }
      }
    },
    width: {
      immediate: true,
      handler(newVal, oldVal) {
        if (newVal !== oldVal) {
          switch (newVal) {
            case 'boxed':
              document.body.setAttribute('data-layout-size', 'boxed')
              break
            case 'fluid':
              document.body.setAttribute('data-layout-mode', 'fluid')
              document.body.removeAttribute('data-layout-size')
              document.body.removeAttribute('data-layout-scrollable')
              break
            default:
              document.body.setAttribute('data-layout-mode', 'fluid')
              break
          }
        }
      }
    }
  },
  created() {
    this.iconSidebar()
  },
  methods: {
    onRoutechange() {
      setTimeout(() => {
        if (document.getElementsByClassName('mm-active').length > 0) {
          const currentPosition = document.getElementsByClassName(
            'mm-active'
          )[0].offsetTop
          if (currentPosition > 500)
            if (this.$refs.isSimplebar)
              this.$refs.isSimplebar.value.getScrollElement().scrollTop =
                currentPosition + 300
        }
      }, 300)
    },
    iconSidebar() {
      document.body.classList.add('sidebar-enable vertical-collpsed')
      document.body.setAttribute('data-keep-enlarged', 'true')
      document.body.removeAttribute('data-sidebar-size', 'small')
      document.body.removeAttribute('data-layout-size', 'boxed')
    },
    showSideBar() {
      document.body.classList.add('vertical-collpsed')
    },
    hideSideBar() {
      document.body.classList.remove('vertical-collpsed')
    },

    show() {
      const classname = document.getElementsByClassName('vertical-collpsed')
      if (classname && classname.length > 0) {
        document.body.classList.remove('vertical-collpsed')
      } else {
        document.body.classList.add('vertical-collpsed')
      }
    }
  }
}
</script>

<template>
  <!-- ========== Left Sidebar Start ========== -->
  <div
    class="vertical-menu"
    @click="show"
    @mouseleave="showSideBar"
    @mouseover="hideSideBar"
  >
    <simplebar
      v-if="!isCondensed"
      id="my-element"
      ref="currentMenu"
      :settings="settings"
      class="h-100"
    >
      <SideNav :menu-items="menuItems" />
    </simplebar>

    <simplebar v-else class="h-100">
      <SideNav :menu-items="menuItems" />
    </simplebar>
  </div>
  <!-- Left Sidebar End -->
</template>
