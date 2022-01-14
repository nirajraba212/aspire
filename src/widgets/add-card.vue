<template>
  <div class="modal-mask">
    <div class="modal-wrapper">
        <div class="modal-container">
            <div class="modal-header">
                Add Card
            </div>

            <div class="modal-body">
                <div class="control-row">
                    <label>Holder Name</label>
                    <input type="text" v-model="name"/>
                </div>
            </div>

            <div class="modal-footer">
                <button class="modal-default-button save-button" :disabled="isSaveDisable" @click="onSaveClick" :class="{'disable': isSaveDisable}">Save</button>
                <button class="modal-default-button cancel-button" @click="onSaveClick">Cancel</button>
            </div>
        </div>
    </div>
</div>
</template>
<script>
export default {
  name: 'addCard',
  props: ["acitveTab"],
  components: {},
  data() {
    return {
      hideBody: false,
      name: ""
    };
  },
  computed: {
    isSaveDisable() {
        return this.name.trim().length == 0;
    }
  },
  methods: {
    onSaveClick() {
        try {
            let data = localStorage.getItem("cardsData");
            data = JSON.parse(data);
            let payload = {
                id: data.data.length + 1,
                name: this.name,
                accountNumber: this.generateAccountNumber(data.data).toString(),
                bankName: null,
                number: this.generateCardNumber(data.data).toString(),
                expireDate: this.generateExpireDate(),
                type: (this.acitveTab == 0) ? "personal" : "company",
                cvv: this.generateCVV().toString(),
                isFreeze: false,
                availableBalance: 0,
                billingAddress: null,
                transactions: []
            };
            this.$emit("submit", payload);
        } catch(e){
            console.log(e);
        }
    },
    generateAccountNumber(data) {
        let accountNumber = Math.floor(1234567891 + Math.random() * 900000);
        if(data.filter(x => x.accountNumber == accountNumber)) {
            return this.generateCardNumber(data);
        } else {
            return accountNumber;
        }        
    },
    generateCardNumber(data) {
        let number = Math.floor(1234567891233456 + Math.random() * 900000);
        if(data.filter(x => x.number == number).length > 0) {
            return this.generateCardNumber(data);
        } else {
            return number;
        }        
    },
    generateExpireDate() {
      var dt = new Date();
      return new Date(dt.setFullYear(dt.getFullYear() + 3)); 
    },
    generateCVV(){
        return Math.floor(123 + Math.random() * 900);
    }
  }
}
</script>
<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}
.modal-mask .modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}
.modal-mask .modal-wrapper .modal-container {
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}
.modal-mask .modal-container .modal-header {
    color: #01D167;
    font-size: 20px;
    border-bottom: 1px solid #F5F5F5;
}
.modal-mask .modal-container .modal-body {
  margin: 20px 0;
}
.modal-mask .modal-container .modal-body .control-row label {
  font-weight: bold;
  margin: 2% 2%;
}
.modal-mask .modal-container .modal-body .control-row input{
    line-height: 35px;
    width: 100%;
    padding-left: 10px;
    border: 1px solid #F5F5F5;
    border-radius: 10px;
}
.modal-mask .modal-container .modal-footer {
    height: 35px;
}
.modal-mask .modal-container .modal-footer .modal-default-button.disable {
    opacity: 50%;
}
.modal-mask .modal-container .modal-footer .modal-default-button {
    border: none;
    color: white;
    padding: 10px 32px;
    text-align: center;
    display: inline-block;
    border-radius: 5px;
    margin-left: 10px;
    cursor: pointer;
    float: right;
}
.modal-mask .modal-container .modal-footer .save-button {
    background-color: #4CAF50;
}
.modal-mask .modal-container .modal-footer .cancel-button {
    background-color: #f44336;
}

</style> 