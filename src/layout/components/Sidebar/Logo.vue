<template>
  <div class="logo">
    <!-- <transition name="fade">
      <span v-if="isCollapse" class="logo_title is-bold " key="0" :class="{'is-text':!type,'is-img':type}">
        <template v-if="type">
          <img :src="website.logo" height="50" />
        </template>
        <template v-else>
          {{website.logo}}
        </template>
      </span>
        </transition>-->
    <transition-group name="fade">
      <template>
        <span key="1" class="logo_title">
          <img :src="('/static/images/nuwa.svg')" width="78">
          <span class="logo-text">機器人管理</span>
        </span>
        <!-- <span class="logo_subtitle" key="2">{{website.author}}</span> -->
      </template>
    </transition-group>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  name: 'Logo',
  props: ['isCollapse'],
  data() {
    return {}
  },
  created() {},
  computed: {
    ...mapGetters(['website']),
    type: function(val) {
      return this.website.logo.indexOf('static') !== -1
    }
  },
  methods: {}
}
</script>

<style scoped="scoped" lang="scss">
.fade-leave-active {
    transition: opacity 0.2s;
}
.fade-enter-active {
    transition: opacity 2.5s;
}
.fade-enter,
.fade-leave-to {
    opacity: 0;
}
.logo {
    margin-top: 25px;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    // width: 230px;
    height: 50px;
    // line-height: 64px;
    background: white;
    color: #fdfdfd;
    text-align: center;
    font-size: 20px;
    font-weight: 600;
    overflow: hidden;
    box-sizing: border-box;
}
.logo_title {
    // padding: 0 5px 0 0;
    color: #333;
    font-size: 18px;
    font-weight: normal;
    &.is-bold {
        font-weight: 700;
    }
    img {
        vertical-align: middle;
    }
}
.logo-text {
    position: relative;
    top: 2px;
    margin-left: 3px;
}
.is-text {
    position: absolute;
    top: 0;
    // left: 20px;
}
.is-img {
    position: absolute;
    // top: 10px;
    // left: 10px;
}
.logo_subtitle {
    font-size: 16px;
    padding-top: 5px;
}
</style>
