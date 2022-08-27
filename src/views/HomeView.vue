<!-- eslint-disable max-len -->
<template>
    <div class="main-container">
        <Navbar />
        <div class="home-content">
            <div class="leading-section">
                <div class="filters-section">
                    <div class="filters-section-one">
                        <div class="show-option">Sorty By:</div>
                        <div class="show-all-option">
                            <span @click="refreshCatalogue" @keydown="refreshCatalogue"> Show All </span>
                        </div>
                    </div>

                    <div class="filters-section-two">
                        <div>
                            Type:
                            <select v-model="filterValue" aria-label="wine-type">
                                <option disabled value="">Type</option>
                                <option value="white">White</option>
                                <option value="red">Red</option>
                                <option value="sparkling">Sparkling</option>
                            </select>
                        </div>

                        <div>
                            Vintage Status:
                            <select v-model="vintageValue" aria-label="vintage-status">
                                <option disabled value="">Select Status</option>
                                <option value="vintage">Vintage</option>
                                <option value="">Any</option>
                            </select>
                        </div>
                    </div>

                    <div class="filters-section-three">
                        <div>
                            Price:
                            <select v-model="catalogueOrder" aria-label="price-order">
                                <option disabled value="">Sort by:</option>
                                <option value="asc">Price: Low to High</option>
                                <option value="dsc">Price: High to Low</option>
                            </select>
                        </div>
                    </div>
                </div>

                <div class="order-section">
                    <div class="order-info">Order info</div>

                    <div class="bottle-count-section">
                        <h3>Bottles: {{ totalBottles }}</h3>
                        <h3>Cases: {{ totalCases }}</h3>
                    </div>

                    <div class="cart-section">
                        <div class="cart-section__upper">
                            <div class="cart-items">
                                <!-- cart modal  -->
                                <vue-final-modal
                                    v-model="showCartModal"
                                    classes="modal-container"
                                    content-class="modal-content"
                                >
                                    <button class="modal__close" @click="showCartModal = false">X</button>
                                    <span class="modal__title">Your Cart</span>
                                    <div class="modal__content">
                                        <div class="modal__item" v-for="item in cartContents" :key="item.no">
                                            <img class="modal__item-image" :src="item.imgUrl" alt="product" />
                                            <div class="modal__item-text-meta">
                                                <span>{{ item.wineNumber }}</span>
                                                <span>{{ item.wineName }}</span>
                                                <span>bottle: {{ item.bottleQuantity }}</span>
                                                <span>case: {{ item.caseQuantity }}</span>
                                            </div>
                                        </div>
                                    </div>
                                </vue-final-modal>

                                <button @click="showCartModal = true" @keydown="showCartModal = true">
                                    View Cart
                                </button>
                            </div>

                            <div class="checkout-area">
                                <button class="btn empty-btn" @click="emptyCart">Empty Cart</button>
                            </div>
                        </div>

                        <div class="cart-section__lower">
                            <div class="empty-cart">
                                <span v-if="totalBalance">Ksh. &nbsp;</span>
                                {{ totalBalance }}
                            </div>

                            <div class="checkout-area">
                                <button class="btn checkout-btn" @click="checkoutItemsToggle">Check Out</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="products-list">
                <ProductCard
                    v-for="item in filteredCatalogue"
                    :key="item.no"
                    :wine-number="item.no"
                    :wine-name="item.name"
                    :img-name="item.image"
                    :bottle-price="item.cost.bottle"
                    :case-price="item.cost.case"
                    :wine-description="item.details"
                    @add-product="addToCart"
                />
            </div>
        </div>
        <Footer />
    </div>
</template>

<script>
import swal2 from 'sweetalert2';
import { VueFinalModal } from 'vue-final-modal';
import Navbar from '@/components/UI/navbar-component.vue';
import Footer from '@/components/UI/footer-component.vue';
import ProductCard from '@/components/Home/product-card.vue';

export default {
    name: 'HomeView',
    components: {
        Navbar,
        Footer,
        ProductCard,
        VueFinalModal,
    },

    data() {
        return {
            showCartModal: false,
            catalogue: [],
            filterValue: '',
            vintageValue: '',
            catalogueOrder: '',
            value: '',
            cartContents: [],
            totalBottles: '',
            totalCases: '',
            totalBalance: '',
        };
    },

    async created() {
        this.getCatalogue();
    },

    computed: {
        filteredCatalogue() {
            if (!this.filterValue) {
                return this.catalogue;
            }

            // eslint-disable-next-line max-len
            return this.catalogue.filter((x) => x.tags.includes(this.filterValue));
        },
    },

    watch: {
        vintageValue(value) {
            this.vintageValue = value;
            if (value === 'vintage') {
                // eslint-disable-next-line max-len
                this.catalogue = this.filteredCatalogue.filter((x) => x.tags.includes(value));
            }
            if (!value) {
                this.getCatalogue();
                return this.catalogue;
            }

            return this.filteredCatalogue;
        },

        catalogueOrder(value) {
            this.catalogueOrder = value;
            if (value === 'asc') {
                this.filteredCatalogue.sort((a, b) => a.cost.bottle - b.cost.bottle);
            } else {
                this.filteredCatalogue.sort((a, b) => b.cost.bottle - a.cost.bottle);
            }
        },
    },

    methods: {
        async getCatalogue() {
            const response = await fetch('https://storage.googleapis.com/wineshop-assets/wine-shop.json');
            const data = await response.json();

            this.catalogue = data;
            this.filterdCatalogue = this.catalogue;
        },

        invalidInputMessage(message) {
            swal2.fire({
                title: message,
                width: 400,
                padding: '2em',
                icon: 'warning',
                backdrop: 'rgba(231,76,60,0.3)',
            });
        },

        addToCart(itemQuantityInfo) {
            if (itemQuantityInfo.bottleQuantity <= 0 && itemQuantityInfo.caseQuantity <= 0) {
                this.invalidInputMessage('Add valid a number.');
                return;
            }

            this.cartContents.push(itemQuantityInfo);

            this.getTotalBottles();
            this.getTotalCases();
            this.getBalance();
        },

        getTotalBottles() {
            let sum = 0;

            this.cartContents.forEach((element) => {
                const parsedQuantity = parseInt(element.bottleQuantity, 10);

                sum += parsedQuantity;
            });

            this.totalBottles = sum;
        },

        getTotalCases() {
            let sum = 0;

            this.cartContents.forEach((element) => {
                const parsedQuantity = parseInt(element.caseQuantity, 10);

                sum += parsedQuantity;
            });

            this.totalCases = sum;
        },

        getBalance() {
            let sum = 0;

            this.cartContents.forEach((element) => {
                const bottleAmount = element.bottleQuantity * element.bottlePrice;
                const caseAmount = element.caseQuantity * element.casePrice;

                const itemsTotal = bottleAmount + caseAmount;
                sum += itemsTotal;
            });

            this.totalBalance = sum.toFixed(2);
        },

        checkoutItemsToggle() {
            if (this.cartContents.length === 0) {
                swal2.fire({
                    title: 'Cart is Empty',
                    icon: 'warning',
                    button: 'Close',
                });
                return;
            }
            this.$router.push({
                name: 'cart',
                params: {
                    cartItems: JSON.stringify(this.cartContents),
                    balance: this.totalBalance,
                },
            });
        },

        emptyCart() {
            this.cartContents.length = 0;
            this.totalBottles = 0;
            this.totalCases = 0;
            this.totalBalance = 0;
        },

        refreshCatalogue() {
            this.getCatalogue();
        },
    },
};
</script>

<style scoped>
.home-content {
  width: 80vw;
  margin: auto;
}

.leading-section {
  display: flex;
  margin-top: 2em;
}

.filters-section {
  flex: 1;
  display: flex;
  flex-direction: column;
  font-size: 0.8em;
  font-weight: 300;
  padding: 1em;
}

.filters-section-one {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}

.show-option {
  flex: 1;
  padding: 0.5em;
  background: var(--transparent-gray);
}

.show-all-option {
  flex: 2;
  display: flex;
  justify-content: flex-end;
  padding: 0.5em;
  color: gray;
}

.filters-section-two {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.filters-section-two,
.filters-section-three {
  padding: 1em;
  background: var(--transparent-gray);
}

.order-section {
  flex: 2;
  display: flex;
  flex-direction: row;
  color: var(--off-black);
  font-family: var(--iowan);
}

.order-info {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 0.8em;
  font-family: var(--roboto);
  margin: 1px;
  background: var(--transparent-gray);
}

.bottle-count-section {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-size: 1.5em;
  font-weight: 400;
  margin: 1px;
  background: var(--transparent-gray);
}

.cart-section {
  flex: 3;
  display: flex;
  flex-direction: column;
  margin: 1px;
}

.cart-section__upper,
.cart-section__lower {
  flex: 1;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  margin: 1px;
  background: var(--transparent-gray);
}

.empty-cart {
  flex: 1;
  font-size: 1.5em;
  font-weight: 600;
  display: flex;
  justify-content: center;
}

.cart-items {
  flex: 1;
  font-family: var(--roboto);
  font-size: 0.7em;
  font-weight: 400;
  display: flex;
  justify-content: center;
}

.checkout-area {
  flex: 1;
  display: flex;
  justify-content: center;
}

.checkout-btn,
.empty-btn {
  font-family: var(--roboto);
  padding: 1em;
}

.checkout-btn {
  background-color: var(--off-black);
  color: var(--white);
}

.empty-btn {
  background-color: var(--gray);
  color: var(--text-gray);
}

.products-list {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  padding: 1em 0;
}

@media (max-width: 780px) {
  .order-section {
    position: fixed;
    bottom: 0;
    background-color: var(--light-gray);
    width: 80%;
  }

  .order-info,
  .bottle-count-section {
    display: none;
  }

  .btn {
    padding: 0.7em;
    margin: 2px;
  }
}

/* modal styles  */

::v-deep .modal-container {
  display: flex;
  justify-content: center;
  align-items: center;
}
::v-deep .modal-content {
  position: relative;
  display: flex;
  flex-direction: column;
  margin: 0 1rem;
  padding: 1rem;
  border: 1px solid #e2e8f0;
  border-radius: 0.25rem;
  background: #fff;
}
.modal__title {
  margin: 0 2rem 0 0;
  font-size: 1.5rem;
  font-weight: 700;
}
.modal__close {
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
}

.modal__item {
  display: flex;
  justify-content: flex-start;
  padding: 5px;
  border-radius: 5px;
  border: 1px solid var(--transparent-dark);
}

.modal__item-text-meta {
  display: flex;
  font-size: 0.7em;
}

.modal__item-text-meta > span {
  margin: auto 10px;
  width: auto;
}

.modal__item-image {
  width: 15px;
  height: 40px;
}
</style>
