<template>
    <div class="col-sm-6 col-md-4">
        <div class="panel panel-success">
            <div class="panel-heading">
                <h3 class="panel-title">
                    {{stock.name}}
                    <small>(Price: {{stock.price}})</small>
                </h3>
            </div>
            <div class="panel-body row">
                <div class="col-md-6 pull-left">
                    <input type="text" :class="{danger:insufficentFunds}" v-model="quantity" class="form-control" placeholder="Quantity">
                </div>
                <div class="col-md-6 pull-right">
                    <button  @click="buyStock" class="btn btn-success" :disabled="insufficentFunds || +quantity <= 0 || !Number.isInteger(+quantity)" >{{ insufficentFunds ? 'Insufieccent funds' : 'Buy' }}</button>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .danger {
        border: 1px solid red;
    }
</style>

<script>
export default {
    props: ['stock'],
    data() {
        return {
            quantity: 0
        }
    },
    computed: {
        funds() {
            return this.$store.getters.funds;
        },
        insufficentFunds() {
            return +this.quantity * this.stock.price > this.funds;
        }
    },
    methods: {
        buyStock() {
            const order = {
                stockId: this.stock.id,
                stockPrice: this.stock.price,
                quantity: +this.quantity
            };
            console.log(order);
            this.$store.dispatch('buyStock', order);
            this.quantity = 0;
        }
    }
}
</script>