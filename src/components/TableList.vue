<template>
  <div class="table-name-list-wrapper" v-loading.body="loading">
    <div class="table-name-list">
      <div v-for="(table_name,index) in tables" :key="table_name" class="table-item" :class="{ active: isActive(index) }" @click="selectTable(table_name, index)">
        <i class="el-icon-document"></i><span class="table-name">{{ table_name }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  name: 'TableList',
  computed: mapGetters({
    tables: 'tables',
    lastEvaluatedKey: 'lastEvaluatedKey'
  }),
  data () {
    return {
      loading: false,
      realSelectedIndex: this.selectedIndex
    }
  },
  created () {
  },
  methods: {
    selectTable: function (tableName, index) {
      this.$store.dispatch('getHeaders', tableName)

      // this.$store.dispatch('showLoading')
      this.$store.dispatch('setResults', {
        'tableName': tableName
      })
      this.$store.dispatch('setKeys', tableName)
      this.realSelectedIndex = index
      this.$store.dispatch('pushLastEvaluatedKey', this.lastEvaluatedKey)
    },
    isActive: function (index) {
      return index === this.realSelectedIndex
    }
  },
  watch: {
    tables: function (val, oldVal) {
      console.log('new: %s, old: %s', val, oldVal)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.table-name-list {
  padding-bottom: 60px;
}

.table-name-list-wrapper {
  overflow-x: auto;
  overflow-y: auto;
  min-height: 400px;
  height: 100%;
}

.el-icon-menu {

}
.table-item {
  margin: 10px 0;
  white-space:nowrap;
  cursor:pointer;
}

.table-name {
  margin-left: 5px;
}

.table-name:hover {
  color: #58B7FF;
}

.active {
  color: #1D8CE0;
}
</style>
