<template>
  <div class="budget-list-wrap">
    <div id="dynamic-component-demo" class="demo">
      <button
        v-for="tab in tabs"
        v-bind:key="tab"
        v-bind:class="['tab-button', { active: currentTab === tab }]"
        v-on:click="currentTab = tab"
      >
        {{ tab }}
      </button>
    </div>
    <ElCard :header="header">
      <template v-if="!isEmpty">
        <div v-for="(item,index) in filterList" :key="index" class="budget-card">
            <div v-if="item.value > 0"><i class="el-icon-top"></i></div>
            <div v-else-if="item.value < 0"><i class="el-icon-bottom"></i></div>
            <span class="budget-value">{{ item.value }}</span>
            <ElButton type="danger" size="mini" @click="deleteItem(item.id)">Delete</ElButton>
        </div>
      </template>
      <ElAlert v-else type="info" :title="emptyTitle" :closable="false" />
    </ElCard>
    </div>

</template>

<script>
export default {
  name: "BudgetList",
  props: {
    list: {
      type: Object,
      default: () => ({})
    }

  },

  data: () => ({
    header: "Budget List",
    emptyTitle: "Empty List",
    currentTab: "All",
    tabs: ["All", "Income", "Outcome"], currentTabIndex: 0,
  }),
  computed: {
    filterList(){
      if (this.currentTabComponent ==='tab-all') {
        return this.list
      }
      else{
        return Object.values(this.list).filter(item => 'tab-' + item.type.toLowerCase() === 'tab-' + this.currentTab.toLowerCase())}
    },
    isEmpty() {
      return !Object.keys(this.list).length;
    },
    currentTabComponent: function() {
      return "tab-" + this.currentTab.toLowerCase();
    },
  },
  methods: {
    deleteItem(id) {
      this.$emit("deleteItem", id);
      confirm('Do you really want to delete this income/outcome?')
    },
  }

};

</script>

<style scoped>
.budget-list-wrap {
  max-width: 500px;
  margin: auto;
}

.list-item {
  display: flex;
  align-items: center;
  padding: 10px 15px;
}
.budget-value {
  font-weight: bold;
  margin-left: auto;
  margin-right: 20px;
}

.el-icon-top {
  color: green;
}
.el-icon-bottom {
  color: red;
}

.tab-button {
  padding: 6px 10px;
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
  border: 1px solid #ccc;
  cursor: pointer;
  background: #f0f0f0;
  margin-bottom: -1px;
  margin-right: -1px;
}
.tab-button:hover {
  background: #e0e0e0;
}
.tab-button.active {
  background: #e0e0e0;
}
.tab {
  border: 1px solid #ccc;
  padding: 10px;
}

.budget-card  {
  display: flex;
  flex-flow: row nowrap;
  align-items: center;
  padding: 10px;
}
</style>