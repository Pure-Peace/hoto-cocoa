<template>
  <div>
    <div
      v-if="fixed"
      id="top-navbar-placeholder"
      :style="topNavbarHeight"
    />
    <div
      id="top-navbar-box"
      ref="topNavbarBox"
      :style="topNavbarHeight"
      :class="topNavbarBoxClass"
    >
      <div id="top-navbar-content">
        <div
          v-if="themeTopNavbarFixed"
          class="logo-box"
          :style="themeOpenSider ? 'width: 210px;' : 'width: 80px;'"
        >
          <div
            class="logo-box-content"
          >
            <div>
              <img
                style="height: 50px; margin: 0 4px;"
                src="@/assets/biglogo_min.png"
              >
            </div>
            <div
              style="transition: .2s ease; text-align: center;"
              class="logo-text"
              :style="logoTextStyle"
            >
              osu!Kafuu
            </div>
          </div>
        </div>
        <div
          class="sidebar-open-control-button"
          :style="miniSizePage ? 'margin-left: 0px;' : 'margin-left: 25px;'"
          @click="sidebarOpenSwitch"
        >
          <a-icon :type="themeOpenSider ? 'menu-fold' : 'menu-unfold'" />
        </div>
        <div
          v-if="!miniSizePage"
          id="top-navbar-menu"
        >
          <!--div
            class="search-box"
            :style="openSearchBox ? 'width: 180px;' : 'width: 30px;'"
          >
            <a-icon
              type="search"
              class="search-button"
              @click="handleSearch"
            />
            <input
              type="text"
              class="search-input"
              placeholder="寻找玩家..."
            >
          </div-->

          <div
            style="margin-left: auto;"
            @click="jumpTo('Switcher')"
            class="outline-button"
          >
            切换器
          </div>
          <div
            @click="jumpTo('Login')"
            class="primary-button"
          >
            sign in~
          </div>
        </div>
        <div
          v-else
          id="top-navbar-menu"
        >
          <div
            class="primary-button"
            style="margin-left: auto; font-size: 18px;"
            @click="openTopNavbarMenu"
          >
            <a-icon
              style="color: #FFFFFF;"
              type="bars"
            />
          </div>
          <div
            v-show="openTopNavbarFixedMenu"
            id="top-navbar-fixed-menu"
            :style="`top: ${height}px;`"
          >
            <div
              @click="jumpTo('Login')"
              class="top-navbar-fixed-menu-item"
            >
              注册 / 登录
            </div>
            <div
              @click="jumpTo('Switcher')"
              class="top-navbar-fixed-menu-item"
            >
              切换器
            </div>
            <div
              @click="jumpTo('Introduce')"
              class="top-navbar-fixed-menu-item"
            >
              介绍
            </div>
          </div>
          <div
            @click="openTopNavbarFixedMenu = false"
            v-show="openTopNavbarFixedMenu"
            class="top-navbar-fixed-menu-mask"
            :style="`top: ${height}px; height: calc(100vh - ${height}px);`"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  props: {
    fixed: {
      type: Boolean,
      default: false
    },
    height: {
      type: Number,
      default: 75
    }
  },
  data () {
    return {
      openSearchBox: true,
      openTopNavbarFixedMenu: false
    }
  },
  watch: {
    miniSizePage: {
      handler: function (val) {
        this.openSearchBox = !val
        this.openTopNavbarFixedMenu = false
      }
    }
  },
  methods: {
    jumpTo (name) {
      this.$router.push({ name: name })
      if (this.openTopNavbarFixedMenu) this.openTopNavbarFixedMenu = false
    },
    sidebarOpenSwitch () {
      this.$store.commit('setThemeOpenSider')
    },
    sidebarFixedSwitch () {
      this.$store.commit('setThemeSiderFixed')
    },
    topNavbarFixedSwitch () {
      this.$store.commit('setThemeTopNavbarFixed')
    },
    sidebarShowSwitch () {
      this.$store.commit('setThemeShowSider')
    },
    handleSearch () {

    },
    openTopNavbarMenu () {
      this.openTopNavbarFixedMenu = !this.openTopNavbarFixedMenu
    }
  },
  computed: {
    ...mapGetters(['themeOpenSider', 'pageWidth', 'isMobile', 'themeTopNavbarFixed']),
    topNavbarBoxClass () {
      return this.fixed ? 'top-navbar-box-fixed' : ''
    },
    topNavbarHeight () {
      return `height: ${this.height}px; `
    },
    miniSizePage () {
      const mini = (this.pageWidth < 700) || this.isMobile
      return mini
    },
    logoTextStyle () {
      return this.themeOpenSider ? 'opacity: 1; width: 90px;' : 'opacity: 0; width: 0;'
    }
  }
}
</script>

<style soped lang="less">
  .logo-box {
    height: 75px;
    font-size: 16px;
    font-weight: bold;
    user-select: none;
    overflow: hidden;
    display: flex;
    cursor: pointer;
    justify-content: center;
    background-color: #3949AB;
    transition: .4s ease;
    /*border-right: 1px dashed rgba(0, 0, 0, .1);*/
  }

  .logo-box-content {
    display: flex;
    align-items: center;
  }

  .search-button {
    cursor: pointer;
    user-select: none;
    transition: .4s ease;
  }

  .search-button:hover {
    filter: brightness(2) opacity(.9);
  }

  .search-button:active {
    filter: brightness(.9);
  }

  .search-input {
    font-size: 12px;
    padding: 0 10px;
    width: 70%;
  }

  .search-box {
    background-color: #141414;
    padding: 6px 8px;
    border-radius: 4px;
    margin: 0 6px;
    overflow: hidden;
    white-space:nowrap;
    width: 180px;
    transition: .4s ease;
    border: 1px solid #FAFAFA;
  }

  .outline-button {
    box-sizing: border-box;
    border: 2px solid #C5CAE9;
    padding: 7px 20px;
    border-radius: 4px;
    font-size: 12px;
    font-weight: bold;
    cursor: pointer;
    user-select: none;
    transition: .4s ease;
    margin: 0 6px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .outline-button:hover {
    background-color:rgba(0, 0, 0, .015);
    filter: brightness(1.1) opacity(.9);
  }

  .outline-button:active {
    background-color:rgba(0, 0, 0, .1);
    filter: brightness(.9);
  }

  .primary-button {
    background-color: #4C62DB;
    padding: 8px 20px;
    border-radius: 4px;
    font-size: 12px;
    font-weight: bold;
    cursor: pointer;
    user-select: none;
    transition: .4s ease;
    margin: 0 6px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .primary-button:hover {
    filter: brightness(1.1);
  }

  .primary-button:active {
    filter: brightness(.9);
  }

  #top-navbar-box {
    display: flex;
    align-items: center;
    background-color: #3949AB;
    width: 100%;
  }

  .top-navbar-box-fixed {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 50;
  }

  .top-navbar-fixed-menu-item {
    padding: 14px;
    transition: .2s ease;
    border-radius: 4px;
    user-select: none;
    cursor: pointer;
    background-color: rgba(0, 0, 0, .015);
    margin: 10px 0;
  }

  .top-navbar-fixed-menu-item:hover {
    filter: brightness(1.1);
  }

  .top-navbar-fixed-menu-item:active {
    filter: brightness(.9);
  }

  #top-navbar-fixed-menu {
    position: absolute;
    width: 100%;
    left: 0;
    background-color: #141414;
    transition: .4s ease;
    padding: 10px;
    z-index: 15;
    border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  }

  .top-navbar-fixed-menu-mask {
    width: 100%;
    position: fixed;
    left: 0;
    background-color: rgba(0,0,0,0.7);
    user-select: none;
    cursor: pointer;
    z-index: 10;
  }

  #top-navbar-placeholder {
    flex: 1;
  }

  #top-navbar-content {
    flex: 1;
    height: 100%;
    display: flex;
    align-items: center;
    padding: 10px 0;
  }

  #top-navbar-menu {
    flex: 1;
    display: flex;
    align-items: center;
    padding: 0 15px;
  }

  .sidebar-open-control-button {
    width: 41px;
    height: 41px;
    border-radius: 50%;
    font-size: 18px;
    padding: 8px;
    cursor: pointer;
    transition: .4s ease;
    color: #FAFAFA;
    user-select: none;
    text-align: center;
  }

  .sidebar-open-control-button:hover {
    background-color: rgba(0, 0, 0, .2);
  }

  .sidebar-open-control-button:active {
    background-color: rgba(0, 0, 0, .4);
  }

  input::-webkit-input-placeholder {
    color: #D3D3D3;
  }

  input:-moz-placeholder {
    color: #D3D3D3;
  }

  input::-moz-placeholder {
    color: #D3D3D3;
  }

  input:-ms-input-placeholder {
    color: #D3D3D3;
  }

</style>
