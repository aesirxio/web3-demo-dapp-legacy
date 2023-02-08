<script>
import ProductAdd from "../components/PIM/ProductAdd.vue";
import Product from "../components/PIM/Product.vue";

import Concordium from "../web3/concordium";
import Swal from "sweetalert2";

export default {
  components: {
    ProductAdd,
    Product,
  },
  mounted() {
    this.concordium = new Concordium();
  },
  data() {
    return {
      account: "",
      concordium: null,
      products: [],
    };
  },
  methods: {
    connect: async function () {
      try {
        this.account = await this.concordium.accountConnect();

        this.productList();
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
      await this.axios.post(
        "http://localhost/product/v1",
        JSON.stringify({
          sku: sku,
          name: name,
          description: description,
          block: trx,
          token: token,
        }),
        {
          headers: {
            "Content-Type": "application/json",
          },
        }
      );
      this.productList();
    },
    async productList() {
      const nonce = (
        await this.axios.get(
          "http://localhost/account/v1/" + this.account + "/nonce"
        )
      ).data.nonce;
      const signedNonce = await this.concordium.signMessage(String(nonce));
      const products = await this.axios.get(
        "http://localhost/product/v1/" +
          this.account +
          "?signature=" +
          signedNonce
      );
      this.products = products.data;
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

  <br />

  <h3>My products</h3>
  <br />

  <div class="grid grid-cols-2 gap-2">
    <Product
      v-for="p in products"
      :key="p.token"
      :sku="p.sku"
      :name="p.name"
      :description="p.description"
      :block="p.block"
      :token="p.token"
    />
  </div>
</template>

