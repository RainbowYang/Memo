<template>
  <el-card shadow="hover">
    <template #header>
      <div id="operation">
        <ItemEditor :originItem="item" style="display:inline-block"/>
        <ItemRemover @remove="remove(item)"/>
      </div>
    </template>
    <el-descriptions :column="1" border>
      <el-descriptions-item>
        <template #label><i class="el-icon-user"></i>
          {{ $t('Author') }}
        </template>
        <div id="authors" v-if="item.author">
          <el-tag v-for="author in item.author.split(',')" :key="author" type="success">{{ author }}</el-tag>
        </div>
        <el-tag v-else type="info">{{ $t('Unknown') }}</el-tag>
      </el-descriptions-item>

      <el-descriptions-item>
        <template #label><i class="el-icon-location-outline"></i>
          {{ $t('Progress') }}
        </template>
        <el-progress :stroke-width="24" :percentage="progressPercentage"/>
      </el-descriptions-item>

      <el-descriptions-item>
        <template #label><i class="el-icon-link"></i>
          {{ $t('Link') }}
        </template>
        <el-link v-if="item.URL" :href="item.URL" type="primary">{{ item.URL }}</el-link>
        <el-tag v-else type="info">{{ $t('Unknown') }}</el-tag>
      </el-descriptions-item>

    </el-descriptions>
  </el-card>
</template>

<script>
import ItemEditor from '@/components/ItemEditor'
import ItemRemover from '@/components/ItemRemover'

export default {
  name: 'Item',
  props: ['item'],
  inject: ['removeItem'],
  components: { ItemEditor, ItemRemover },
  data () {
    return {
      dialogVisible: false,
    }
  },
  computed: {
    progressPercentage () {
      return this.item.progress || (this.item.state === '2' ? 100 : 0)
    },
  },
  methods: {
    remove () {
      this.removeItem(this.item)
    },
  },
}
</script>

<style scoped>
#operation {
  display: flex;
  justify-content: space-around;
}

.el-tag {
  margin-right: 10px;
}

.el-progress {
  width: 300px;
}

.el-row {
  margin-bottom: 20px;
}

.el-row:last-child {
  margin-bottom: 0;
}
</style>
