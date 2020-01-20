<template>
  <div class="dashboard-editor-container">
    <h3>Avatars / Roles</h3>
    <el-button @click="udpateRole('Owner')">Owner</el-button>
    <el-button @click="udpateRole('Admin')">Admin</el-button>
    <el-button @click="udpateRole('Member')">Member</el-button>
    <el-button @click="udpateRole('User')">User</el-button>
    <el-button @click="udpateRole('Student')">Student</el-button>
    <p />

    <hr>
    <br>

    <h3>Teams</h3>
    <el-button @click="udpateTeam('teams')">Teams</el-button>
    <el-button @click="udpateTeam('empty')">Empty</el-button>
    <p />
    <hr>
    <br>

    <h3>Message Alert</h3>
    <el-button @click="udpateMsgBox('msg')">Messages</el-button>
    <el-button @click="udpateMsgBox('empty')">Empty</el-button>
    <p />
    <hr>
    <br>

    <h3>Theme - {{ value }}</h3>
    <el-select v-model="value" placeholder="Select" @change="handleSelected">
      <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value" />
    </el-select>

    <p />
  </div>
</template>

<script>

import Cookies from 'js-cookie'

export default {
  name: 'DashboardAdmin',
  components: {

  },
  data() {
    return {
      options: [
        {
          value: 'light',
          label: 'Light'
        },
        {
          value: 'dark',
          label: 'Dark'
        }
      ],
      value: Cookies.get('ThemeType') || 'light'
    }
  },
  careted() {
    Cookies.set('ThemeType', this.value)
  },
  methods: {
    handleSelected(val) {
      Cookies.set('ThemeType', val)
      this.value = val
      location.reload()
    },

    udpateRole(roleName) {
      // alert(roleName)
      this.$root.$emit('roleChanged', roleName.toLowerCase())
    },
    udpateTeam(teamsList) {
      this.$root.$emit('teamChanged', teamsList.toLowerCase())
    },
    udpateMsgBox(msgs) {
      this.$root.$emit('msgBoxChanged', msgs.toLowerCase())
    }
  }
}
</script>

<style lang="scss" scoped>
.dashboard-editor-container {
  padding: 32px;
  // background-color: rgb(240, 242, 245);
  position: relative;

  .github-corner {
    position: absolute;
    top: 0px;
    border: 0;
    right: 0;
  }

  .chart-wrapper {
    background: #fff;
    padding: 16px 16px 0;
    margin-bottom: 32px;
  }
}

@media (max-width: 1024px) {
  .chart-wrapper {
    padding: 8px;
  }
}
</style>
