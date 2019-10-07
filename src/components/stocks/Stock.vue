<template>
    <div class="col-sm-6 col-md-4">
        <div class="panel panel-success">
            <div class="panel-heading">
                <h3 class="panel-title">
                    {{ stock.name }}
                    <small>(Price: {{ stock.price }})</small>
                </h3>
            </div>
            <div class="panel-body">
                <div class="pull-left">
                    <input
                            type="number"
                            class="form-control"
                            placeholder="Quantity"
                            v-model="quantity"
                            :class="{danger: insufficientFunds}"
                    >
                </div>
                <div class="pull-right">
                    <button
                            class="btn btn-success"
                            @click="buyStock"
                            :disabled="insufficientFunds || quantity <= 0"
                    >{{ insufficientFunds ? 'Insufficient Funds' : 'Buy' }}
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>


<style>
    .danger {
        color: red;
    }
</style>

<script>
    export default {
        props: ['stock'],
        data(){
            return {
                quantity: 0
            }
        }, 
        computed: {
            funds(){
                return this.$store.getters.funds;
            },
            insufficientFunds() {
                   console.log("over zero?", this.quantity <= 0 )
                console.log( "is Number? ",Number.isInteger(this.quantity));
                console.log(this.quantity * this.stock.price);
                console.log("fund " ,this.funds)
                console.log(this.quantity);
                return this.quantity * this.stock.price > this.funds;
            }
        }, 
        methods: {
            buyStock(){
                const order = {
                    stockId: this.stock.id,
                    stockPrice: this.stock.price,
                    quantity: this.quantity
                };
                this.$store.dispatch('buyStock', order);
                this.quantity = 0;
            }
        }
    }
</script>