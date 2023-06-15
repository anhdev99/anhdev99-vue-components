<script>
import { MetisMenu } from 'metismenujs'

/**
 * Sidenav component
 */
export default {
  props: {
    menuItems: {
      type: Array,
      required: false
    }
  },
  data() {
    return {
      menuData: null
    }
  },
  mounted: function() {
    if (document.getElementById('side-menu')) MetisMenu.attach('#side-menu')
    var links = document.getElementsByClassName('side-nav-link-ref')
    var matchingMenuItem = null
    const paths = []

    for (var i = 0; i < links.length; i++) {
      paths.push(links[i]['pathname'])
    }
    var itemIndex = paths.indexOf(window.location.pathname)
    if (itemIndex === -1) {
      const strIndex = window.location.pathname.lastIndexOf('/')
      const item = window.location.pathname.substr(0, strIndex).toString()
      matchingMenuItem = links[paths.indexOf(item)]
    } else {
      matchingMenuItem = links[itemIndex]
    }
    if (matchingMenuItem) {
      matchingMenuItem.classList.add('active')
      var parent = matchingMenuItem.parentElement

      /**
       * TODO: This is hard coded way of expading/activating parent menu dropdown and working till level 3.
       * We should come up with non hard coded approach
       */
      if (parent) {
        parent.classList.add('mm-active')
        const parent2 = parent.parentElement.closest('ul')
        if (parent2 && parent2.id !== 'side-menu') {
          parent2.classList.add('mm-show')

          const parent3 = parent2.parentElement
          if (parent3) {
            parent3.classList.add('mm-active')
            var childAnchor = parent3.querySelector('.has-arrow')
            var childDropdown = parent3.querySelector('.has-dropdown')
            if (childAnchor) childAnchor.classList.add('mm-active')
            if (childDropdown) childDropdown.classList.add('mm-active')

            const parent4 = parent3.parentElement
            if (parent4 && parent4.id !== 'side-menu') {
              parent4.classList.add('mm-show')
              const parent5 = parent4.parentElement
              if (parent5 && parent5.id !== 'side-menu') {
                parent5.classList.add('mm-active')
                const childanchor = parent5.querySelector('.is-parent')
                if (childanchor && parent5.id !== 'side-menu') {
                  childanchor.classList.add('mm-active')
                }
              }
            }
          }
        }
      }
    }
  },
  methods: {
    /**
     * Returns true or false if given menu item has child or not
     * @param item menuItem
     */
    hasItems(item) {
      return item.subItems !== undefined ? item.subItems.length > 0 : false
    },

    toggleMenu(event) {
      event.currentTarget.nextElementSibling.classList.toggle('mm-show')
    }
  }
}
</script>

<template>
  <!--- Sidemenu -->
  <div id="sidebar-menu">
    <!-- Left Menu Start -->
    <ul id="side-menu" class="metismenu list-unstyled">
      <li class="menu-title-primary">
        <a class="cs-menu-title" href="">
          <div class="cs-icon-menu">
            <img
              alt="nhan-su"
              height="30"
              src="/assets/images/icon/icon_qlns.png"
            />
          </div>
          <span class="text-uppercase ms-2 font-weight-bold text-dark"
            >Quản lý nhân sự</span
          >
        </a>
      </li>
      <template v-for="item in menuItems">
        <li v-if="item.isTitle" :key="item.id" class="menu-title">
          {{ item.label }}
        </li>
        <li v-if="!item.isTitle && !item.isLayout" :key="item.id">
          <a
            v-if="hasItems(item)"
            :class="{ 'has-arrow': !item.badge, 'has-dropdown': item.badge }"
            class="is-parent"
            href="javascript:void(0);"
          >
            <i v-if="item.icon" :class="`bx ${item.icon}`" />
            <span>{{ item.label }}</span>
            <span
              v-if="item.badge"
              :class="`badge rounded-pill bg-${item.badge.variant} float-end`"
              >{{ item.badge.text }}</span
            >
          </a>

          <router-link
            v-if="!hasItems(item)"
            :to="item.link"
            class="side-nav-link-ref side-nav-link"
          >
            <i v-if="item.icon" :class="`bx ${item.icon}`" />
            <span>{{ item.label }}</span>
            <span
              v-if="item.badge"
              :class="`badge rounded-pill bg-${item.badge.variant} float-end`"
              >{{ item.badge.text }}</span
            >
          </router-link>

          <ul v-if="hasItems(item)" aria-expanded="false" class="sub-menu">
            <li v-for="(subitem, index) of item.subItems" :key="index">
              <router-link
                v-if="!hasItems(subitem)"
                :to="subitem.link"
                class="side-nav-link-ref"
              >
                {{ subitem.label }}
              </router-link>
              <a
                v-if="hasItems(subitem)"
                class="side-nav-link-a-ref has-arrow"
                href="javascript:void(0);"
                >{{ subitem.label }}</a
              >
              <ul
                v-if="hasItems(subitem)"
                aria-expanded="false"
                class="sub-menu mm-collapse"
              >
                <li
                  v-for="(subSubitem, index1) of subitem.subItems"
                  :key="index1"
                >
                  <router-link :to="subSubitem.link" class="side-nav-link-ref">
                    {{ subSubitem.label }}
                  </router-link>
                </li>
              </ul>
            </li>
          </ul>
        </li>
      </template>
    </ul>
  </div>
</template>
