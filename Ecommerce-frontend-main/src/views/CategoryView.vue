<template>
  <div class="page-category"></div>
  <div class="columns is-multiline">
    <div class="column is-12">
      <h2 class="is-size-2 has-text-centered">{{ category.name }}</h2>
    </div>

    <ProductBox
      v-for="product in category.products"
      v-bind:key="product.id"
      v-bind:product="product"
    />
  </div>
</template>

<script>
import axios from "axios";
import { toast } from "bulma-toast";
import ProductBox from "@/components/ProductBox.vue";

export default {
  name: "CategoryView",
  components: {
    ProductBox,
  },
  data() {
    return {
      category: {
        products: [],
      },
    };
  },
  mounted() {
    this.getCategory();
  },
  watch: {
    $route(to, from) {
      console.log(to, from);
      if (to.name === "Category") {
        this.getCategory();
      }
    },
  },
  methods: {
    async getCategory() {
      const categorySlug = this.$route.params.category_slug;

      this.$store.commit("setIsLoading", true);

      axios
        .get(`/api/v1/products/${categorySlug}`)
        .then((response) => {
          this.category = response.data;

          document.title = this.category.name + " | Djackets";
        })
        .catch((error) => {
          console.log(error);

          toast({
            message: "Error loading category",
            type: "is-danger",
            position: "bottom-right",
            dismissible: true,
            pauseOnHover: true,
            duration: 2000,
          });
        });

      this.$store.commit("setIsLoading", false);
    },
  },
};
</script>

<style scoped></style>
