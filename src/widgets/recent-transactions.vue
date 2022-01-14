<template>
  <div class="section">
    <div class="section-header">
        <div class="section-header-details" @click="hideBody = !hideBody">
            <span class="icon recent-trans-icon small"></span>
            <label class="caption">Recent Transactions</label>
            <span class="icon small" :class="{'down-icon': hideBody,'up-icon': !hideBody}"></span>
        </div>
    </div>
    <div class="section-body" :class="{ 'hide' : hideBody }" v-if="recentTransactionsList.length">
        <div v-for="(item,index) in recentTransactionsList" :key="index" class="list-item">
            <span class="catagory"><span class="catagory-bg"><span class="icon medium" :class="item.category + '-icon'"></span></span></span>
            <div class="item-details">
                <span class="name">{{item.name}}</span>
                <span class="date">{{item.date}}</span>
                <span class="desc"><span class="icon-bg"><span class="icon business-icon small"></span></span>{{item.desc}}</span>
            </div>
            <label class="item-amount" :class="item.type"><span class="icon currancy-icon">{{item.type == "credit" ? "+ S$ " : "- S$ "}}</span><span class="amount">{{item.amount}}</span></label>
        </div>
    </div>
    <div class="section-footer" :class="{ 'hide' : hideBody }" v-if="list.length > 4">
        <label class="caption">View all card transactions</label>
    </div>
  </div>
</template>
<script>
export default {
  name: 'recentTransactions',
  components: {},
  data() {
      return {
          hideBody: false,
          list: []
      };
  },
  computed: {
    recentTransactionsList() {
      if(this.list.length > 0){
        return (this.list.length > 4) ? this.list.slice(0, 4) : this.list;
      } else {
        return [];
      }
    }
  },
  methods: {
    loadData(data) {
      this.list = data.transactions;
    }
  }
}
</script>
<style>

</style> 