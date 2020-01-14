<template>
  <el-scrollbar wrap-class="scrollbar-wrapper">
    <div>
      <logo :is-collapse="isCollapse" />
      <div class="instruction">{{ $t('sidebar.title') }}</div>
      <el-menu
        :default-active="$route.path"
        :background-color="variables.menuBg"
        :text-color="variables.menuText"
        :active-text-color="variables.menuActiveText"
        mode="vertical"
      >
        <sidebar-item
          v-for="route in permission_routes"
          :key="route.path"
          :item="route"
          :base-path="route.path"
        />
      </el-menu>

      <div class="footer-info">
        <el-button icon="question-circle-solid" disabled type="text" @click="nav('helper')">
          <svg-icon icon-class="question-circle-solid" />
          {{ $t('sidebar.helper') }}
        </el-button>
        <br>
        <el-button
          icon="comment-dots-solid"
          type="text"
          style="color:#333"
          @click="feedback('feedback')"
        >
          <svg-icon icon-class="comment-dots-solid" />
          {{ $t('sidebar.feedback') }}
        </el-button>

        <div class="center ver-font">Version: {{ version }}</div>
      </div>
    </div>
  </el-scrollbar>
</template>

<script>
import { mapGetters } from 'vuex'
import SidebarItem from './SidebarItem'
import variables from '@/styles/variables.scss'
import logo from './logo'
import { version } from '@/../package.json'

export default {
  components: { SidebarItem, logo },
  data() {
    return {
      version
    }
  },
  computed: {
    ...mapGetters([
      'permission_routes',
      'sidebar'
    ]),
    variables() {
      return variables
    },
    isCollapse() {
      return !this.sidebar.opened
    }
    // permission_routers_menu1() {
    //   return this.permission_routers.filter(x => x.menu === undefined)
    // },
    // permission_routers_menu2() {
    //   return this.permission_routers.filter(x => x.menu === 2)
    // }
  },
  methods: {
    feedback() {
      this.$root.$emit('feedbackShow', true)
    }
  }
}
</script>

<style lang="scss" scoped>
.footer-info {
    font-size: 14px;
    position: absolute;
    bottom: 0;
    height: auto;
    // margin-bottom: 15px;
    width: 100%;
    // text-align: center;
    color: #333;
    // left: 15px;
    .center {
        text-align: center;
    }
    padding: 15px;
}

.instruction {
    font-size: 14px;
    color: gray;
    margin: 50px 0 0 15px;
    -webkit-transition: opacity 0.5s ease-in;
    -moz-transition: opacity 0.5s ease-in;
    -o-transition: opacity 0.5s ease-in;
    transition: opacity 0.5s ease-in;
    opacity: 1;
}

.instruction2 {
    margin: 10px 0 0 15px;
}
.hide {
    opacity: 0;
}

.ver-font{
  font-size: 9px;
  margin-top: 15px;
  color: rgba(51, 51, 51, 0.8);
}
</style>
