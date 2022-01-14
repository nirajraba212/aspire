<template>
  <carousel :per-page="1" :mouse-drag="true" @page-change="pageChange">
    <slide v-for="(card,index) in list" :ref="'cards' + index" :key="index" :data-index="index" :data-name="card.id">
      <div class="card" :class="{'freeze-card': card.isFreeze }">
        <div class="icon company-icon aspire-logo-icon-white large"></div>
        <div class="icon visa-icon visa-white-icon large"></div>
        <div class="number">
            <div class="number-card">
              <span v-if="!isShowNumber">
                <span v-for="index in 4" :key="index" class="dot"></span>          
              </span>
              <span v-if="isShowNumber">{{formatCardNumber(card.number).substring(0,4)}}</span>
            </div>
            <div class="number-card">
              <span v-if="!isShowNumber">
                <span v-for="index in 4" :key="index" class="dot"></span>          
              </span>
              <span v-if="isShowNumber">{{formatCardNumber(card.number).substring(4,8)}}</span>
            </div>
            <div class="number-card">
              <span v-if="!isShowNumber">
                <span v-for="index in 4" :key="index" class="dot"></span>          
              </span>
              <span v-if="isShowNumber">{{formatCardNumber(card.number).substring(8,12)}}</span>
            </div>
            <div class="number-card"><span>{{formatCardNumber(card.number).substring(12,16)}}</span></div>
        </div>
        <div class="name">{{card.name}}</div>
        <div class="date"><label>Thru:</label> <span class="value"> {{formatExpireDate(card.expireDate)}}</span></div>
        <div class="cvv"><label>CVV:</label> <span class="cvv-value" :class="{'isShowCVV' : isShowCVV}">{{formatCVV(card.cvv)}}</span></div>
      </div>
    </slide>
  </carousel>
</template>
<script>

import { Carousel, Slide } from 'vue-carousel';

export default {
  name: 'cardSlides',
  components: {
    Carousel,
    Slide
  },
  props: ["isShowNumber","cardsData", "isShowCVV"],
  data() {
      return {
        cardList: [],
        currentTab: 0
      };
  },
  computed: {
    list() {
      return this.cardList
    }
  },
  methods: {
    loadData(data) {
      this.cardList = [];
      this.cardList = data;
    },
    formatCardNumber (value) {
      if(value) {
        const regex = /^(\d{0,4})(\d{0,4})(\d{0,4})(\d{0,4})$/g
        const onlyNumbers = value.replace(/[^\d]/g, '')

        if(this.isShowNumber) {
          return value;
        } else {
          return onlyNumbers.replace(regex, (regex, $1, $2, $3, $4) =>
            ["....", "****", "****", $4].filter(group => !!group).join('')
          )
        }
      } else {
        return "";
      }
    },
    formatExpireDate(date) {
      var dateObj = new Date(date);
      var month = dateObj.getUTCMonth() + 1;
      var year = dateObj.getUTCFullYear();
      return  (month.toString().length == 1 ? "0" + month.toString() : month) + "/" + year.toString().substring(2,4);
    },
    formatCVV(value) {
      return this.isShowCVV ? value : "***";
    },
    pageChange(index) {
      let obj = (this.list && this.list.length > 0 && this.list[index]) ? this.list[index] : null;
      delete obj.transactions;
      this.$root.$emit("onSelectCard", obj);
    }
  }
}
</script>
<style>
.VueCarousel, 
.VueCarousel-wrapper {
  height: 100%;
}
.VueCarousel-inner {
  height: 100% !important;
}
.VueCarousel-slide .card{
  background-color: #01D167;
  border-radius: 16px;
}
.VueCarousel-slide .card.freeze-card {
  background-color: #808080;
}
.VueCarousel-dot-container, .VueCarousel-dot-container button {
  margin: 0 !important;
}
.VueCarousel-dot-container button{
    padding: 0px 10px !important;
}
.card {
  position: relative;
  height: 100%;
  border-radius: 25px;
  overflow: hidden;
}
.card .number,
.card .name,
.card .date,
.card .cvv {
  letter-spacing: 2px;
  color: #FFFFFF;
  font-weight: bold;
}
.card .name {
  font-size: 24px;
  left: 40px;
  bottom: 125px;
  position: absolute;
}
.card .number {
  left: 40px;
  bottom: 85px;
}
.card .number .number-card {
  padding-right: 25px;
  display: inline-block;
}
.card .number .number-card:last-child {
  padding-right: 0px;
}
.card .number .number-card span {
  display: inline-block;
}
.card .number .number-card .dot {
  height: 9px;
  width: 9px;
  background-color: #FFFFFF;
  border-radius: 50%;
  display: inline-block;
  margin-right: 3px;
}
.card .date {
  left: 40px;
}
.card .cvv {  
  left: 170px;
}
.card .date, .card .cvv {
  font-size: 13px;
  bottom: 50px;
  display: flex;
}
.card .cvv .cvv-value {
  font-size: 24px;
  line-height: 24px;
  height: 20px;
  padding-left: 5px;
}
.card .cvv .cvv-value.isShowCVV {
  font-size: 13px;
}
.card .logo img,
.card .number,
.card .name,
.card .date,
.card .cvv {
  position: absolute; /* All items inside card should have absolute position */
}
.card .company-icon {
  top: 35px;
  right: 35px;
  width: 100px;
  height: 30px;
  position: absolute;
}
.card .visa-icon {
  bottom: 30px;
  right: 35px;
  width: 80px;
  height: 30px;
  position: absolute;
}
</style>