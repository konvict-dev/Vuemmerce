<template>
  <div class="top-margin bottom-margin">
    <breadcrumbs-component :items="path"/>
    <pagination-component :items="products">
      <template slot="itemsOnPage"
                slot-scope="{ itemsOnPage: products }">
        <categories-navigation  class='column is-2 is-pulled-left is-desktop is-narrow' :id='id' />
        <div class="columns is-centered is-multiline">
          <div class="card column is-one-quarter"
               v-for="product in products"
               :key="product.id">
            <products-component :product="product"></products-component>
          </div>
          <div class="section" v-if="products.length === 0">
            <p>{{ noProductLabel }}</p>
          </div>
        </div>
      </template>
    </pagination-component>
  </div>
</template>

<script>
import ProductsComponent from '../Products';
import BreadcrumbsComponent from '../Breadcrumbs'
import PaginationComponent from '../pagination/Pagination'
import CategoriesNavigation from '../categories_nav/CategoriesNavigation';

export default {
  name: 'products-list-component',

  components: {
    ProductsComponent,
    BreadcrumbsComponent,
    PaginationComponent,
    CategoriesNavigation,
  },

  props: {
    id: {
      type: [Number, String],
      required: true
    }
  },

  data () {
    return {
      noProductLabel: 'No product found in this category',
    };
  },

  computed: {
    category () {
      return this.$store.getters.getCategoryById(this.$route.params.id);
    },
    products () {
      return this.$store.state.products.filter(product => String(product.category) === String(this.$route.params.id));
    },
    path () {
      return [
        {
          text: 'Home',
          to: { path: '/' }
        },
        {
          text: this.category.title,
        }
      ]
    }
  }

};
</script>

<style lang="scss" scoped>
  .top-margin {
    margin-top: 10px;
  }
  .bottom-margin {
    margin-bottom: 10px;
  }
  .card {
    margin: 10px;
  }
</style>
