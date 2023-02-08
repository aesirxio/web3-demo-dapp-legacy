<script>
import ProductAdd from "../components/PIM/ProductAdd.vue";

import Concordium from "../web3/concordium";
import Swal from "sweetalert2";

export default {
  components: {
    ProductAdd,
  },
  mounted() {
    this.concordium = new Concordium();
  },
  data() {
    return {
      account: "",
      concordium: null,
    };
  },
  methods: {
    connect: async function () {
      try {
        this.account = await this.concordium.accountConnect();
      } catch (e) {
        Swal.fire({
          title: "No connection",
          text:
            "Cannot connect to Concordium account.  Please review that the browser wallet is installed. = " +
            e.message,
          icon: "error",
        });
      }
    },
    async productAdd(sku, name, description) {
      const token = await this.concordium.getNextNFT();
      const trx = await this.concordium.mintNFT(this.account, token);
    },
  },
  computed: {
    buttonConnectDisabled: function () {
      if (this.account === "") return null;
      return "disabled";
    },
  },
};
</script>

<template>
  <h1>Web3 Demo</h1>

  <div class="my-2">
    Connected account:
    <div class="badge">
      <div v-if="account === ''">No Concordium account connected</div>
      {{ account }}
    </div>
  </div>
  <button
    class="rounded-full bg-black text-white"
    :class="buttonConnectDisabled"
    :disabled="buttonConnectDisabled"
    v-on:click="this.connect()"
  >
    Connect to Concordium account
  </button>

  <br />
  <ProductAdd v-if="account" v-on:add="productAdd" />
</template>
