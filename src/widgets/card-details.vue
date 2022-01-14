<template>
  <div class="section">
    <div class="section-header">
        <div class="section-header-details" @click="hideBody = !hideBody">
            <span class="icon card-detail-icon small"></span>
            <label class="caption">Card details</label>
            <span class="icon small" :class="{'down-icon': hideBody,'up-icon': !hideBody}"></span>
        </div>
    </div>
    <div class="section-body"  :class="{ 'hide' : hideBody }" v-if="cardDetails">
      <div class="card-detail-row holder-name"><label>Holder Name</label><span>{{cardDetails.name}}</span></div>
      <div class="card-detail-row bank-name"><label>Bank Name</label><span>{{cardDetails.bankName}}</span></div>
      <div class="card-detail-row account-number"><label>Account Number</label><span>{{cardDetails.accountNumber}}</span></div>
      <div class="card-detail-row billing-address"><label>Billing Address</label><span>{{cardDetails.billingAddress}}</span></div>
      <div class="card-detail-row date-cvv">
        <div class="expire-date"><label>Expired Date</label><span>{{formatExpireDate(cardDetails.expireDate)}}</span></div>
        <div class="cvv-number"><label>CVV</label><span>{{cardDetails.cvv}}</span></div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'cardDetails',
  components: {},
  data() {
    return {
      hideBody: false,
      cardDetails: null
    };
  },
  methods: {
    loadData(data) {
      this.cardDetails = data;
    },
    formatExpireDate(date) {
      var dateObj = new Date(date);
      var month = dateObj.getUTCMonth() + 1; //months from 1-12
      var year = dateObj.getUTCFullYear();
      return  (month.toString().length == 1 ? "0" + month.toString() : month) + "/" + year.toString();
    },
  }
}
</script>
<style>
.section-body {
  padding: 10px;
}
.card-detail-row label {
  font-weight: bold;
  margin: 2% 2%;
}
.card-detail-row span{
  line-height: 35px;
    /* width: 96%; */
    display: block;
    position: relative;
    padding-left: 10px;
    height: 35px;
    margin: 0% 2% 2% 2%;
    border: 1px solid #F5F5F5;
    border-radius: 10px;
}
.card-detail-row.billing-address span {
  height: auto;
  word-break: break-word;
}
.card-detail-row.date-cvv {
  display: inline-flex;
  width: 98%;
}
.card-detail-row.date-cvv div {
  width: 50%;
  position: relative;
}
.expire-date {
  margin-right: 2%;
}
</style> 