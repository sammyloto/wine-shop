<template>
    <div class="parent-container">
        <div class="product-content">
            <div class="image-col">
                <img :src="imgUrl" alt="wine-bottle" />
            </div>

            <div class="info-col">
                <div class="info-col-title">
                    <h2 class="title-number">{{ wineNumber }}</h2>
                    <h3 class="title-name">{{ wineName }}</h3>
                </div>
                <div class="info-col-counter">
                    <div class="bottle">
                        <p>Bottle</p>
                        <p>KSh. {{ bottlePrice }}</p>

                        <span>
                            <input
                                type="text"
                                class="quantity-field"
                                v-model="bottleQuantity"
                                aria-label="bottle-qty"
                            />
                            QTY
                        </span>
                    </div>

                    <div class="case">
                        <p>Case</p>
                        <p>KSh. {{ casePrice }}</p>

                        <span>
                            <input
                                type="text"
                                class="quantity-field"
                                v-model="caseQuantity"
                                aria-label="case-qty"
                            />
                            QTY
                        </span>
                    </div>
                </div>
                <div class="info-col-button-section">
                    <!-- eslint-disable-next-line max-len-->
                    <button
                        class="btn details-button"
                        @click="getProductDetails(wineName, wineDescription, imgUrl)"
                    >
                        Details
                    </button>
                    <button
                        class="btn cart-button"
                        @click="
                            $emit('add-product', {
                                wineName,
                                wineNumber,
                                bottlePrice,
                                casePrice,
                                bottleQuantity,
                                caseQuantity,
                                imgUrl,
                            })
                        "
                    >
                        Add to Cart
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import swal2 from 'sweetalert2';

export default {
    name: 'ProductCard',
    props: {
        wineNumber: String,
        wineName: String,
        imgName: String,
        bottlePrice: Number,
        casePrice: Number,
        wineDescription: String,
        method: { type: Function },
    },
    data() {
        return {
            caseQuantity: 0,
            bottleQuantity: 0,
            imgUrl: 'https://storage.googleapis.com/wineshop-assets/wine-bottles/',
        };
    },

    created() {
        this.imgUrl = `https://storage.googleapis.com/wineshop-assets/wine-bottles/${this.imgName}`;
    },

    methods: {
        getProductDetails(name, details, image) {
            swal2.fire({
                title: `${name}`,
                text: `${details}`,
                imageUrl: `${image}`,
                imageWidth: 50,
                imageHeight: 100,
                imageAlt: `${name}`,
            });
        },
    },
};
</script>

<style scoped>
.parent-container {
  flex: 0 0 33.333333%;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1em;
  min-height: 400px;
  margin-bottom: -1px;
  border-bottom: 2px solid var(--dark-gray);
  border-bottom-style: dotted;
  font-family: var(--iowan);
  font-weight: 600;
}
.product-content {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}

.image-col {
  flex: 1;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}

.info-col {
  flex: 1;
}

.info-col-title {
  color: var(--dark-gray) !important;
}

.title-number {
  font-size: 2.8em;
}

.title-name {
  font-size: 1.5em;
  margin-top: -10px;
}

img {
  height: 220px;
  width: 60px;
}

.info-col-counter {
  display: flex;
  flex-direction: row;
  font-size: 1em;
  font-weight: 500;
}

.bottle,
.case {
  flex: 1;
  padding: 0.5em;
}

.bottle {
  border-right: 1px solid var(--black);
  border-right-style: dotted;
}

.quantity-field {
  width: 30%;
}

.info-col-button-section {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin-top: 0.5em;
}

.details-button {
  background-color: var(--gray);
  color: var(--text-gray);
}

.cart-button {
  background-color: var(--off-black);
  color: var(--white);
}

.details-button:hover {
  background-color: var(--gray-hover);
  transition: 300ms;
}

.cart-button:hover {
  background-color: var(--off-black-hover);
  transition: 300ms;
}

@media (max-width: 780px) {
  .parent-container {
    flex: 0 0 100%;
  }
}
</style>
