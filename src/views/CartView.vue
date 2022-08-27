<!-- eslint-disable max-len -->
<template>
    <div class="main-container">
        <Navbar />

        <div class="main-container-content">
            <DeliveryColumn />
            <CartColumn />
        </div>

        <Footer />
    </div>
</template>

<script>
import Navbar from '@/components/UI/navbar-component.vue';
import Footer from '@/components/UI/footer-component.vue';
import DeliveryColumn from '@/components/Cart/delivery-form.vue';
import CartColumn from '@/components/Cart/cart-contents.vue';

export default {
    name: 'CartView',
    components: {
        Navbar,
        Footer,
        DeliveryColumn,
        CartColumn,
    },

    data() {
        return {
            data() {
                return {
                    testValue: 'testy',
                };
            },
        };
    },
};
</script>

<style scoped>
.main-container {
  width: 80vw;
  margin: 0 auto;
}

.main-container-content {
  display: flex;
  min-height: 85vh;
}

@media (max-width: 700px) {
  .main-container-content {
      flex-direction: column-reverse;
  }
  }
</style>
