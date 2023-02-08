<template>
  <div class="border-2 border-gray-900 p-2">
    <h3>Add new product</h3>
    <div class="grid grid-cols-2 gap-2">
      <input
        type="text"
        class="form-input px-4 py-3 rounded-full"
        v-model="sku"
        placeholder="SKU"
      />
      <input
        type="text"
        class="form-input px-4 py-3 rounded-full"
        v-model="name"
        placeholder="Product Name"
      />
      <textarea
        class="col-span-full form-input px-4 py-3 rounded-full"
        placeholder="Product description"
        v-model="description"
      ></textarea>
      <button
        class="col-span-full rounded-full bg-black text-white"
        v-on:click="this.add()"
      >
        Mint new product NFT
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sku: "",
      name: "",
      description: "",
    };
  },
  methods: {
    add: function () {
      this.$emit("add", this.sku, this.name, this.description);
    },
  },
};
</script>