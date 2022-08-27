<!-- eslint-disable max-len -->
<!-- eslint-disable vuejs-accessibility/label-has-for -->
<template>
    <div class="column-parent">
        <div class="container">
            <div class="title-area">
                <img src="@/assets/images/delivery-truck.png" alt="truck" />
                <h3>Delivery Details</h3>
                <p>
                    <!-- eslint-disable-next-line max-len -->
                    Kindly enter details on where you would like your order delivered using the form below.
                </p>
            </div>

            <form class="form-area" @submit.prevent="submitOrder" @submit="submitOrder">
                <div class="name-section">
                    <span>
                        <label for="fname">First name</label>
                        <br />
                        <input type="text" v-model="firstName" id="fname" placeholder="Jon" required />
                    </span>

                    <span>
                        <label for="sname">Second name</label>
                        <br />
                        <input type="text" v-model="secondName" id="sname" placeholder="Doe" />
                    </span>
                </div>
                <div class="mid-section">
                    <span>
                        <label for="email">E-mail</label>
                        <br />
                        <input type="email" v-model="email" id="email" placeholder="user@user.com" required />
                    </span>

                    <span>
                        <label for="phone">Phone</label>
                        <br />
                        <input
                            type="text"
                            v-model="phoneNumber"
                            id="phone"
                            placeholder="+254 7XX XXX XXX"
                            required
                        />
                    </span>

                    <span>
                        <label for="address">Address / Street</label>
                        <br />
                        <input type="text" v-model="address" id="address" required />
                    </span>

                    <span>
                        <label for="building">Building </label>
                        <br />
                        <input type="text" v-model="building" id="building" required />
                    </span>

                    <span>
                        <label for="instructions">Add Instruction Details</label>
                        <br />
                        <textarea
                            name=""
                            v-model="instructions"
                            id="instructions"
                            cols="30"
                            rows="10"
                        ></textarea>
                    </span>
                </div>

                <div class="button-section">
                    <button class="btn btn-submit">Submit Order</button>
                </div>
            </form>
        </div>
    </div>
</template>

<script>
import swal2 from 'sweetalert2';

export default {
    name: 'DeliveryForm',

    data() {
        return {
            firstName: '',
            secondName: '',
            email: '',
            phoneNumber: '',
            address: '',
            building: '',
        };
    },

    methods: {
        submitOrder() {
            swal2.fire({
                icon: 'success',
                title: 'Order Sucessful!',
                text: `
                      Name: ${this.firstName} ${this.secondName} \n
                      Contact: ${this.phoneNumber} \n
                      Address: ${this.address} \n
                      Building: ${this.building}\n
                      Amount: Kshs: ${this.$route.params.balance}
                      `,
            });
        },
    },
};
</script>

<style scoped>
@import url("./styles.css");

.container {
  display: flex;
  flex-direction: column;
  height: auto;
}

.title-area {
  color: var(--off-black);
  font-size: 1em;
  margin: 1em 0.5em;
  padding: 0.5em;
}

.title-area > p {
  font-size: 0.8em;
  color: var(--text-gray);
}

.title-area > img {
  width: 40px;
  height: 40px;
}

.form-area {
  color: var(--text-gray);
  font-size: 1em;
  font-weight: 600;
  margin: 0 0.5em;
}

.name-section {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.button-section {
  padding: 0.5em;
}

.btn-submit:hover {
  color: rgb(92, 92, 253);
  background: var(--transparent-gray);
  transition: 200ms;
}

span {
  flex: 1;
  display: block;
  margin: 0.5em;
}

label {
  font-size: 0.9em;
  font-weight: 400;
}

input[type="text"],
input[type="email"],
textarea {
  width: 99%;
  padding: 0.3em 0.3em;
  border-radius: 3px;
  border: 1px solid var(--text-gray);
}

@media (max-width: 706px) {
  .container {
    width: 100%;
    font-size: 0.8em;
  }
}
</style>
