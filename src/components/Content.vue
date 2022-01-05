<template>
  <el-main id="main">
    <el-tabs id="main-tabs" v-model="activeType" tab-position="left">

      <el-tab-pane v-for="type in types"
                   :label="$t(type)" :name="type" :key="type">
        <el-tabs v-model="activeState" type="border-card">
          <el-tab-pane v-for="(state,index) in allStates[type]"
                       :label="$t(state)" :name="index+''" :key="state">
            <el-collapse accordion style="min-height: 300px">
              <el-collapse-item v-for="item in filterItems" :key="item.title">
                <template #title>
                  <el-tag :class="typedIcon">
                    <span v-html="$t('localeTitle', [item.title])"/>
                  </el-tag>
                </template>
                <Item :item="item"></Item>
              </el-collapse-item>
              <el-empty v-if="!filterItems.length" :description="$t('Nothing')"></el-empty>
            </el-collapse>
            <el-input v-model="inputTitle" :placeholder="$t('AddSth')"
                      @change="addItem(inputItem);inputTitle=''"></el-input>
          </el-tab-pane>
        </el-tabs>
      </el-tab-pane>
      <el-tab-pane :label="$t('Operations')">
        <el-tabs type="border-card">
          <el-tab-pane :label="$t('Export')">
            <div style="display: flex">
              <pre>{{ exportItems }}</pre>
              <el-button @click="exportData()">{{ $t('copy') }}</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane :label="$t('Import')">
            <div style="display: flex">
              <el-input type="primary" v-model="input"></el-input>
              <el-button @click="importData()">{{ $t('OK') }}</el-button>
            </div>
          </el-tab-pane>
        </el-tabs>
      </el-tab-pane>
    </el-tabs>
  </el-main>
</template>

<script>
import Item from '@/components/Item'

export default {
  name: 'Content',
  components: { Item },
  data () {
    return {
      inputTitle: '',
      types: ['book', 'movie'],
      allStates: {
        book: ['to read', 'reading', 'has read'],
        movie: ['to watch', 'watching', 'has watched'],
      },
      activeType: 'book',
      activeState: '1',
      items: [],
      input: '',
    }
  },
  created () {
    this.items = localStorage.getItem('items') ?
        JSON.parse(localStorage.getItem('items'))
        : []
  },
  watch: {
    items (items) {
      localStorage.setItem('items', JSON.stringify(items))
    },
  },
  computed: {
    inputItem () {
      return {
        title: this.inputTitle,
        state: this.activeState,
        type: this.activeType,
      }
    },
    typedIcon () {
      switch (this.activeType) {
        case 'book':
          return 'el-icon-notebook-2'
        case 'movie':
          return 'el-icon-video-camera'
        default:
          return ''
      }
    },
    filterItems () {
      return this.items.filter(({ type, state }) =>
          type === this.activeType && state === this.activeState)
    },
    exportItems () {
      return btoa(JSON.stringify(this.items))
    },
  },
  methods: {
    exportData () {
      function copyToClip (content) {
        var aux = document.createElement('input')
        aux.setAttribute('value', content)
        document.body.appendChild(aux)
        aux.select()
        document.execCommand('copy')
        document.body.removeChild(aux)
      }

      copyToClip(this.exportItems)
    },
    importData () {
      Array.prototype.unique = function () {
        return [...new Set(this)]
      }

      this.items = ([...this.items, ...JSON.parse(atob(this.input))]
          .map(it => JSON.stringify(it)))
          .unique()
          .map(it => JSON.parse(it))
    },
    addItem (item) {
      this.items = [...this.items, { ...item, time: Date.now() }]
    },
    removeItem ({ time }) {
      this.items = this.items.filter(item => item.time !== time)
    },
    editItem (originItem, newItem) {
      this.removeItem(originItem)
      this.addItem({ ...originItem, ...newItem })
    },
  },
  provide () {
    return {
      currentStates: this.allStates[this.activeType],

      addItem: this.addItem,
      editItem: this.editItem,
      removeItem: this.removeItem,
    }
  },
}
</script>

<style scoped>
pre, code {
  white-space: pre-wrap;
  overflow-x: scroll;
}

#main {
  padding: 16px 10%;

  /*border-top: 2px solid gray;*/
  /*border-bottom: 2px solid gray;*/

}

#main-tabs {
  box-shadow: rgba(60, 64, 67, 0.3) 0px 1px 2px 0px, rgba(60, 64, 67, 0.15) 0px 1px 3px 1px;

  /*border: 2px solid black;*/

  padding: 12px;
  background-color: white;
  border-radius: 12px;
}

@media (max-width: 988px) {
  #main {
    padding: 16px 5%;
  }
}

@media (max-width: 768px) {
  #main {
    padding: 16px 0;
  }
}
</style>
