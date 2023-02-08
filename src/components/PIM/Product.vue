<template>
  <div class="border-2 border-blue-800 p-2">
    <p>SKU: {{ sku }}</p>
    <p>Name: {{ name }}</p>
    <p>Description: {{ description }}</p>
    <br />
    <p>
      NFT block:
      <a :href="blockLink" target="_blank">{{ block }}</a>
    </p>
    <p>Token: {{ token }}</p>
  </div>
</template>

<script>
export default {
  props: {
    sku: { type: String },
    name: { type: String },
    description: { type: String },
    block: { type: String },
    token: { type: String },
  },
  computed: {
    blockLink: function () {
      return (
        "https://testnet.ccdscan.io/?dcount=1&dentity=transaction&dhash=" +
        this.block
      );
    },
  },
};
</script>