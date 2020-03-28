<template>
    <div class="col-sm-6 col-md-4">
        <div class="panel panel-info">
            <div class="panel-heading">
                <h3 class="panel-title">
                    {{stock.name}}
                    <small>(Price: {{stock.price}}) | {{stock.quantity}}</small>
                </h3>
            </div>
            <div class="panel-body row ">
                <div class="pull-left col-md-6">
                    <input :class="{danger: insufficientQuantity}" type="text" v-model="quantity" class="form-control" placeholder="Quantity">
                </div>
                <div class="pull-right col-md-6">
                    <button @click="sellStock" class="btn btn-success" :disabled="insufficientQuantity || +quantity <= 0 || !Number.isInteger(+quantity)" >{{ insufficientQuantity ? 'Insufficient quantity' : 'Sell' }}</button>
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
import {mapActions} from 'vuex';

export default {
    props: ['stock'],
    data() {
        return {
            quantity: 0
        }
    },
    computed: {
        insufficientQuantity() {
            return this.quantity > this.stock.quantity;
        }
    },
    methods: {
        ...mapActions({
            placeSellOrder: 'sellStock'
        }),
        sellStock() {
            const order = {
                stockId: this.stock.id,
                stockPrice: this.stock.price,
                quantity: this.quantity
            };
            this.placeSellOrder(order);
            this.quantity = 0;
        }
    }
}
</script>