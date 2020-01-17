<template>
  <el-dropdown trigger="click" class="international" @command="handleSetLanguage">
    <div>
      <!-- <svg-icon class-name="international-icon" icon-class="language" /> -->
      <svg-icon icon-class="global" class="icon" />
      {{ displayLang }}<i class="el-icon-arrow-down el-icon--right" />
    </div>
    <el-dropdown-menu slot="dropdown">
      <el-dropdown-item :disabled="language==='zh'" command="zh">
        中文
      </el-dropdown-item>
      <el-dropdown-item :disabled="language==='en'" command="en">
        English
      </el-dropdown-item>
      <el-dropdown-item :disabled="language==='es'" command="es">
        Español
      </el-dropdown-item>
      <el-dropdown-item :disabled="language==='ja'" command="ja">
        日本語
      </el-dropdown-item>
    </el-dropdown-menu>
  </el-dropdown>
</template>

<script>
export default {
  computed: {
    language() {
      return this.$store.getters.language
    },
    displayLang() {
      let langText = 'English'
      switch (this.language) {
        case 'zh':
          langText = '简体中文'
          break
        case 'tw':
          langText = '繁體中文'
          break
        case 'jp':
          langText = '日本語'
          break
      }
      return langText
    }
  },
  methods: {
    handleSetLanguage(lang) {
      this.$i18n.locale = lang
      this.$store.dispatch('app/setLanguage', lang)
      this.$message({
        message: 'Switch Language Success',
        type: 'success'
      })
    }
  }
}
</script>
