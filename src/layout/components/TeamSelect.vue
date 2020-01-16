<template>
  <div>
    <div v-if="teams.length === 0" style="display:inline-block">
      <el-button key="createTeam" plain type="primary" @click="createTeam">{{ $t('team.apply') }}</el-button>
    </div>
    <el-dropdown v-else trigger="click" @command="handleCommand">
      <span class="el-dropdown-link">
        <svg-icon icon-class="group" class="icon" />

        <div style="display:inline-block;min-width:100px;text-align:right">{{ teamName }}</div>

        <i class="el-icon-arrow-down el-icon--right" />
      </span>

      <el-dropdown-menu slot="dropdown">
        <p style="color:#999;font-size:12px; padding: 0 15px;">{{ this.$t('navbar.team.select') }}</p>
        <el-dropdown-item
          v-for="item in teams"
          :key="item.teamId"
          :command="item.teamId"
        >{{ item.name }}</el-dropdown-item>
        <el-dropdown-item divided />
        <el-dropdown-item key="createTeam" command="createTeam">{{ this.$t('navbar.team.apply') }}</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>&nbsp;&nbsp;
  </div>
</template>

<script>
// import { mapGetters } from 'vuex'
// import { getTeamById } from '@/api/team'
// import { getTeamFromUAMS } from '@/utils/uamsInfo'

export default {
  name: 'TeamSelect',
  data() {
    return {
      team: {
        name: '努哇教育集團'
      },
      teamId: 'NB1557903870927',
      teams: []
    }
  },
  computed: {
    // ...mapGetters(['teamId', 'teams']),
    teamName() {
      return this.team.name
    }
  },
  created() {
    this.$root.$on('teamChanged', teamList => {
      if (teamList === 'empty') {
        this.teams = []
      } else {
        this.teams = [
          {
            teamId: 'NB1557903870927',
            name: '努哇教育集團',
            isOwner: false
          },
          {
            teamId: 'NB1563766484392',
            name: 'STEAM 教室',
            isOwner: false
          },
          {
            teamId: 'NB1565172488294',
            name: '個人帳號',
            isOwner: false
          }
        ]
      }
    })

    this.$root.$on('updateUserInfo', teamId => {
      this.getTeamInfo()
    })
    // this.getTeamInfo()
  },
  methods: {
    getTeamName(teamId) {
      const t = this.teams.find(x => x.teamId === teamId)
      return t.name
    },
    createTeam() {
      this.$alert('create team')
    },
    getTeamInfo() {
      //   getTeamById(this.teamId).then(rsp => {
      //     this.team = rsp.data.data
      //   })
    },
    handleCommand(teamId) {
      const vm = this
      if (teamId === 'createTeam') {
        this.$alert('create team')
      } else {
        this.teamId = teamId
        this.team.name = this.getTeamName(teamId)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.el-dropdown-menu {
  box-sizing: content;
  width: 200px;
}
.el-dropdown-menu--medium
  .el-dropdown-menu__item.el-dropdown-menu__item--divided {
  margin: 0 15px;
}

// .item {
//   margin-top: 10px;
//   margin-right: 40px;
// }
</style>
