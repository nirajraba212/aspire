<template>
  <div class="container-section" :class="{'overlay': isloading}">
    <div class="loader"><img src="../assets/icons/loading.gif" alt="Be patient..." /></div>
    <div class="container-header">
        <label class="balance-label">Available balance</label>
        <div class="container-header-details">
            <div class="balance"><span class="currancy">S$</span><span class="value">{{formatAmount}}</span></div>
            <button class="add-button" @click="isAddModal = true"><span class="icon add-button-icon add-icon small"></span>New card</button>
        </div>
    </div>
    <div class="container-body">
        <div class="tabs">
            <div class="tab-links">
                <div class="tab-link" :class="{'active' : acitveTab == 0}">My debit cards</div>
                <div class="tab-link" :class="{'active' : acitveTab == 1}">All company cards</div>
            </div>
            <div class="tab-body">
                <div class="left-panel">
                    <div class="card-slides">
                        <div class="details">
                            <card-slides ref="cardSlides" :cardsData="cardsData" :acitveTab="acitveTab"></card-slides>
                        </div>
                    </div>
                    <div class="action-buttons">
                        <button @click="onFreezeCard"><i class="icon action-icon freeze-icon small"></i><span>{{selectedCard && selectedCard.isFreeze ? "Unfreeze card" : "Freeze card"}}</span></button>
                        <button><i class="icon action-icon spend-limit-icon small"></i><span>Set spend limit</span></button>
                        <button><i class="icon action-icon gpay-icon small"></i><span>Add to GPay</span></button>
                        <button><i class="icon action-icon replace-icon small"></i><span>Replace card</span></button>
                        <button @click="onCancelCardClick"><i class="icon action-icon deactivate-icon small"></i><span>Cancel card</span></button>
                    </div>
                </div>
                <div class="right-panel">
                    <div class="detail-section">
                        <card-details ref="cardDetails" :cardsData="cardsData" :acitveTab="acitveTab"></card-details>
                    </div>
                    <div class="detail-section">
                        <recent-transactions ref="recentTransactions" :cardsData="cardsData" :acitveTab="acitveTab"></recent-transactions>
                    </div>
                </div>
            </div>
        </div>
    </div>
    
    <div class="modal" v-show="isAddModal">
      <add-card @submit="onAddCard" @close="isAddModal = false" :acitveTab="acitveTab"></add-card>
    </div>
    <vue-confirm-dialog></vue-confirm-dialog>
  </div>
</template>
<script>
import cardDetails from "./card-details.vue"
import addCard from "./add-card.vue"
import recentTransactions from "./recent-transactions.vue"
import cardSlides from "./cards-slides.vue"
import data from "../data.json"
export default {
  name: 'container',
  components: {
    cardDetails,
    recentTransactions,
    cardSlides,
    addCard,
  },
  data() {
    return {
        acitveTab: 0,
        cardsData: null,
        isShowNumber: false,
        loaded: false,
        isloading: false,
        isAddModal: false,
        selectedCard: null
    };
  },
  created () {
    this.$root.$on("onLoading",(val) => {
        this.isloading = val;
    });
    this.$root.$on("onSelectCard",(val) => {
        this.selectedCard = val;
        this.loadData();
    });
  },
  mounted() {
    this.loadData();
  },
  computed: {
    formatAmount() {
        return this.selectedCard ? this.selectedCard.availableBalance.toString().replace(/\B(?<!\.\d*)(?=(\d{3})+(?!\d))/g, ",") : "0";
    }
  },
  methods: {
    loadData() {
        this.$root.$emit("onLoading",true);
        this.cardsData = this.getData();
        if(this.$refs.cardDetails && this.$refs.cardDetails.loadData && this.cardsData.length > 0) {
            this.selectedCard = this.selectedCard || this.cardsData[0];
            this.$refs.cardDetails.loadData(this.cardsData.find(x => x.id == this.selectedCard.id));
        }
        if(this.$refs.cardSlides && this.$refs.cardSlides.loadData && this.cardsData.length > 0) {
            this.$refs.cardSlides.loadData(this.cardsData);
        }
        if(this.$refs.recentTransactions && this.$refs.recentTransactions.loadData && this.cardsData.length > 0) {
            this.$refs.recentTransactions.loadData(this.cardsData.find(x => x.id == this.selectedCard.id));
        }
        this.loaded = true;
        this.$root.$emit("onLoading",false);
    },
    getData() {
        try {
            let localData = localStorage.getItem("cardsData");
            if(localData) {
                let data = JSON.parse(localData);
                return data.data;
            } else {
                localStorage.setItem("cardsData", JSON.stringify(data));
                return data.data;
            }
        } catch (e){
            console.log(e);
        }
    },
    onAddCard(cardDetails) {
        this.isAddModal = false;
        try {
            let data = localStorage.getItem("cardsData");
            data = JSON.parse(data);
            data.data.push(cardDetails);
            localStorage.setItem("cardsData", JSON.stringify(data));
            this.loadData();
        }catch (e){
            console.log(e);
        }
    },
    onFreezeCard() {
        try {
            let data = localStorage.getItem("cardsData"); 
            data = JSON.parse(data);
            this.selectedCard.isFreeze = !this.selectedCard.isFreeze;
            data.data.find(x => x.id == this.selectedCard.id).isFreeze = this.selectedCard.isFreeze;
            
            localStorage.setItem("cardsData",JSON.stringify(data));
            this.loadData();
        } catch (e){
            console.log(e);
        }
    },
    onCancelCardClick () {
        this.$confirm(
            {
                title: 'Remove card',
                message: 'Are you sure you want to cancel card?',
                button: { no: 'No', yes: 'Yes'},
                callback: confirm => { if (confirm) { this.removeCard(); } }
            }
        )        
    },
    removeCard() {
        try {
            let data = localStorage.getItem("cardsData"); 
            data = JSON.parse(data);
            let index = data.data.findIndex(x => x.id == this.selectedCard.id);
            this.selectedCard = null;
            data.data.splice(index,1);
            localStorage.setItem("cardsData",JSON.stringify(data));
            this.loadData();
        } catch (e){
            console.log(e);
        }
    }
  }
}
</script>
<style>



</style>
