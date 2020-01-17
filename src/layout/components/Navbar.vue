<template>
  <div class="navbar" :class="!themeType?'light-theme':'dark-theme'">
    <hamburger
      v-if="device === 'mobile'"
      :toggle-click="toggleSideBar"
      :is-active="sidebar.opened"
      class="hamburger-container"
    />
    <!-- <div class="breadcrumb-container">Version: {{version}}</div> -->
    <!-- <breadcrumb class="breadcrumb-container"/> -->
    <div class="right-menu">
      <team-select class="right-menu-item" />&nbsp;&nbsp;
      <message-alert class="right-menu-item" />
      <lang-select class="right-menu-item hover-effect" />
      <el-dropdown class="avatar-container right-menu-item" trigger="click">
        <div class="avatar-wrapper">
          <div class="user-avatar-outer" :class="accountPage?'active':''">
            <div class="user-avatar" :class="stateRole || roles[0]">{{ avatarName }}</div>
          </div>
        </div>
        <el-dropdown-menu slot="dropdown" style="width:150px">
          <router-link to="/profile">
            <el-dropdown-item>
              {{ name }}
              <br>
              <el-tag
                :class="stateRole || roles[0]"
              >{{ $t('common.role.' + (stateRole || roles[0])) }}</el-tag>
            </el-dropdown-item>
          </router-link>
          <el-dropdown-item divided />
          <router-link to="/profile">
            <el-dropdown-item>{{ $t('common.acc') }}</el-dropdown-item>
          </router-link>
          <router-link
            v-if="allowOwnerOp"
            to="/valueadd"
            :disabled="!whateverActivatesThisLink"
            :event="whateverActivatesThisLink ? 'click' : ''"
            :style="whateverActivatesThisLink ? 'cursor: default' : 'cursor: not-allowed'"
          >
            <el-dropdown-item :disabled="!whateverActivatesThisLink">{{ $t('common.valueAdd') }}</el-dropdown-item>
          </router-link>
          <router-link
            v-if="allowOwnerOp"
            to="/billingcenter"
            :disabled="!whateverActivatesThisLink"
            :event="whateverActivatesThisLink ? 'click' : ''"
            :style="whateverActivatesThisLink ? 'cursor: default' : 'cursor: not-allowed'"
          >
            <el-dropdown-item :disabled="!whateverActivatesThisLink">{{ $t('route.billingcenter') }}</el-dropdown-item>
          </router-link>
          <el-dropdown-item divided />
          <el-dropdown-item>
            <div @click="handleLogout">{{ $t('common.logout') }}</div>
          </el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
// import Breadcrumb from '@/components/Breadcrumb'
import Hamburger from '@/components/Hamburger'
import LangSelect from '@/components/LangSelect'
import TeamSelect from './TeamSelect'
import MessageAlert from './MessageAlert'
import store from '@/store'
import ResizeMixin from '../mixin/ResizeHandler'
// import { version } from '@/../package.json'
import Cookies from 'js-cookie'

export default {
  components: {
    // Breadcrumb,
    Hamburger,
    LangSelect,
    TeamSelect,
    MessageAlert
  },
  mixins: [ResizeMixin],
  data() {
    return {
      // version
      whateverActivatesThisLink: false,
      stateRole: null
    }
  },
  created() {
    this.$root.$on('updateUserInfo', () => {
      store.dispatch('GetUserInfo')
    })
    this.$root.$on('roleChanged', roleName => {
      this.stateRole = roleName
    })
  },
  computed: {
    themeType() {
      const theme = Cookies.get('ThemeType') || 'light'
      return theme === 'dark'
    },
    ...mapGetters(['sidebar', 'name', 'avatar', 'device', 'roles']),
    accountPage() {
      return this.$route.path === '/accountCenter'
    },
    allowOp() {
      return this.roles[0] !== 'member'
    },
    allowOwnerOp() {
      return this.roles[0] === 'owner'
    },
    avatarName() {
      let showName = this.name
      const names = this.name.split(' ')
      if (names.length > 1) {
        const first = names[0]
        const last = names[1]
        const english = /^[A-Za-z0-9]*$/
        let charA, charB
        if (english.test(first)) {
          charA = first.charAt(0).toUpperCase()
          charB = last.charAt(0).toUpperCase()
        } else {
          charA = first
          charB = ''
        }
        showName = charA + charB
      }
      return showName
    }
  },
  methods: {
    toggleSideBar() {
      this.$store.dispatch('toggleSideBar')
    },
    // logout () {
    //     this.$store.dispatch('LogOut').then(() => {
    //         location.reload() // In order to re-instantiate the vue-router object to avoid bugs
    //     })
    // },
    handleLogout() {
      this.$confirm(this.$t('personCenter.confirm.logout.text'), '', {
        confirmButtonText: this.$t('common.logout'),
        cancelButtonText: this.$t('common.cancel')
      })
        .then(_ => {
          this.logout()
        })
        .catch(_ => {})
    },
    async logout() {
      await this.$store.dispatch('user/logout')
      this.$router.push(`/login?redirect=${this.$route.fullPath}`)
    }
  }
}
</script>

<style lang="scss" scoped>

.dark-theme {
  @import "src/styles/rms-dark.scss";
}

.light-theme {
  @import "src/styles/rms.scss";
}
</style>

<style rel="stylesheet/scss" lang="scss" scoped>
// .navbar .right-menu .avatar-container .avatar-wrapper {
//     margin-top: 0 !important;
// }
.navbar {
  height: 60px;
  overflow: hidden;

  border-bottom: 1px solid #eee;

  .hamburger-container {
    line-height: 56px;
    height: 100%;
    float: left;
    cursor: pointer;
    transition: background 0.3s;

    &:hover {
      background: rgba(0, 0, 0, 0.025);
    }
  }

  .breadcrumb-container {
    float: left;
    line-height: 60px;
    margin-left: 15px;
    color: #555;
  }

  .errLog-container {
    display: inline-block;
    vertical-align: top;
  }

  .right-menu {
    float: right;
    height: 100%;
    line-height: 60px;

    .active {
      // border-bottom: 3px solid #00b5e2;
      background: rgba(0, 0, 0, 0.1) !important;
    }

    &:focus {
      outline: none;
    }

    .right-menu-item {
      display: inline-block;
      padding: 0 8px;
      height: 100%;
      font-size: 14px;
      // color: #5a5e66;
      vertical-align: text-bottom;

      &.hover-effect {
        cursor: pointer;
        transition: background 0.3s;

        &:hover {
          background: rgba(0, 0, 0, 0.025);
        }
      }
    }

    .avatar-container {
      margin-right: 30px;

      .avatar-wrapper {
        // margin-top: 10px;
        // position: relative;

        .user-avatar-outer {
          position: relative;
          top: 5px;
          width: 50px;
          height: 50px;
          border-radius: 50%;
          background: transparent;
          pointer-events: none;
          &:hover {
            transition: background 0.2s ease-in;
            // background: rgba(0, 0, 0, 0.1);
          }
          transition: background 0.2s ease-out;
          .user-avatar {
            font-size: 18px;
            position: relative;
            top: 5px;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            // background: #00b5e2;
            text-align: center;
            color: white;
            font-weight: bold;
            line-height: 40px;
            left: 5px;
            cursor: pointer;
            pointer-events: auto;
          }
        }

        .el-icon-caret-bottom {
          cursor: pointer;
          position: absolute;
          right: -20px;
          top: 25px;
          font-size: 12px;
        }
      }
    }
  }
}
</style>
<style lang="scss">
.router-link-exact-active.router-link-active {
  li {
    color: #00b5e2;
  }
}
</style>
