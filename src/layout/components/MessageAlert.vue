<template>
  <div>
    <el-badge style="display:inline-block;line-height:0">
      <el-popover placement="bottom" width="200" trigger="click">
        <div v-if="msgs.length === 0" style="text-align:center">
          <div style="padding:1em;">
            <img :src="('/static/images/empty-msg-box.png')" width="78">
            <p class="meg-title">尚未收到任何訊息</p>
          </div>
          <hr>
          <a href="#">
            <p class="msg-nav-to">前往訊息夾</p>
          </a>
        </div>
        <div v-else>
          <div v-for="(m,i) in msgs" :key="i">
            <div class="meg-title">{{ m.title }}</div>
            <p class="etc msg-desc">{{ m.desc }}</p>
            <div class="msg-desc">{{ moment(m.dateTime, "YYYYMMDD").fromNow() }}</div>
            <hr>
          </div>
        </div>
        <el-badge slot="reference">
          <el-badge :is-dot="(msgs.length > 0)" class="item">
            <svg-icon icon-class="bell-solid" class="ic on" style="font-size:2em;" />
          </el-badge>
        </el-badge>
      </el-popover>
    </el-badge>
  </div>
</template>

<script>
// import { mapGetters } from 'vuex'
// import { getTeamById } from '@/api/team'
// import { getTeamFromUAMS } from '@/utils/uamsInfo'
import moment from 'moment'

export default {
  name: 'TeamSelect',
  data() {
    return {
      msgs: [],
      moment
    }
  },
  computed: {
    // ...mapGetters(['teamId', 'teams']),
    teamName() {
      return this.team.name
    }
  },
  created() {
    this.$root.$on('msgBoxChanged', msgs => {
      if (msgs === 'empty') {
        this.msgs = []
      } else {
        this.msgs = [
          {
            title: '凱比二代誕生囉！！',
            desc:
              '就在這個月的最後一天就在這個月的最後一天就在這個月的最後一天',
            dateTime: '2020-01-16T03:00:30.009Z'
          },
          {
            title: '凱比二代誕生囉1！！',
            desc:
              '就在這個月的最後一天就在這個月的最後一天就在這個月的最後一天',
            dateTime: '2020-01-15T03:00:30.009Z'
          },
          {
            title: '凱比二代誕生囉2！！',
            desc:
              '就在這個月的最後一天就在這個月的最後一天就在這個月的最後一天',
            dateTime: '2020-01-14T03:00:30.009Z'
          }
        ]
      }
    })
  },
  methods: {}
}
</script>

<style lang="scss" scoped>
:root {
  --baack: #333;
}

.etc {
  overflow: hidden;
  text-overflow: ellipsis;
  -webkit-line-clamp: 1; // lines
  display: -webkit-box;
  -webkit-box-orient: vertical;
}
.msg-desc {
  opacity: 0.5;
  font-size: 12px;
  font-weight: 300;
  letter-spacing: 0.67px;
  color: var(--black);
}
.meg-title {
  font-size: 12px;
  font-weight: 500;
  letter-spacing: 0.67px;
  color: var(--black);
}
.msg-nav-to {
  font-size: 16px;
  font-weight: 900;
  letter-spacing: 0.89px;
  color: rgba(0, 181, 226, 0.8);
}
</style>
