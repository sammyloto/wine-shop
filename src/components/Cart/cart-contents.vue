<template>
    <div class="column-parent">
        <div class="cart-content">
            <div class="cart-items">
                <h3>Your Order:</h3>
                <div class="item" v-for="item in cartItems" :key="item.no">
                    <img class="item-image" :src="item.imgUrl" alt="product" />
                    <div class="item-text-meta">
                        <span>{{ item.wineNumber }}</span>
                        <span>{{ item.wineName }}</span>
                        <span>bottle: {{ item.bottleQuantity }}</span>
                        <span>case: {{ item.caseQuantity }}</span>
                    </div>
                </div>
            </div>
            <div class="invoice-section">
                <h3>Total Amount:</h3>
                <span class="invoice-amount">Kshs. {{ this.$route.params.balance }}</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CartContents',
    data() {
        return {
            cartItems: [],
        };
    },

    created() {
        this.cartItems = JSON.parse(this.$route.params.cartItems);
    },
};
</script>

<style scoped>
@import url("./styles.css");

.cart-content {
  margin: 1em;
  padding: 0.5em;
  border-radius: 7px;
  background: var(--transparent-white);
  box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 12px;
}
.item {
  display: flex;
  justify-content: flex-start;
  padding: 5px;
  border-radius: 5px;
  border: 1px solid var(--transparent-dark);
}

.item-text-meta {
  display: flex;
  font-size: 0.7em;
}

.item-text-meta > span {
  margin: auto 10px;
  width: auto;
}

.cart-items {
  height: 60vh;
  overflow: hidden;
}

.item-image {
  width: 15px;
  height: 40px;
}

.invoice-section {
  border-top: 1px solid var(--off-black);
  padding-top: 1em;
}

.invoice-amount {
    color: var(--off-black);
    font-family: var(--iowan);
    font-weight: 800;
}
</style>
