<template>
  <el-header id="header">
    <div id="title">
      <span>
        {{ $t('Progress Manager') }}
      </span>
      <el-tooltip class="item" effect="dark" :content="$t('tips')" placement="bottom-start">
        <i class="el-icon-info"></i>
      </el-tooltip>
    </div>
    <LocaleChanger/>
  </el-header>
</template>

<script>
import LocaleChanger from '@/components/LocaleChanger'

export default {
  name: 'Header',
  components: { LocaleChanger },
}
</script>

<style scoped>
#header {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-around;
}

#title span {
  font-size: 36px;
  text-align: center;
}
</style>
