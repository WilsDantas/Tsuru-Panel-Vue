<template>
  <div>
    <ol class="breadcrumb bg-light">
      <li class="breadcrumb-item">
        <router-link :to="{name: 'dashboard'}">Dashboard</router-link>
      </li>
      <li class="breadcrumb-item active" >
        <router-link :to="{name: 'categories.index'}">Category</router-link>
      </li>
      <li class="breadcrumb-item active" >
        <router-link :to="{name: 'categories.edit'}">Edit</router-link>
      </li>
    </ol>
    <div class="card-header">
      <h1>Edit Category</h1>

      <div class="card">
        <div class="card-body">
          <form @submit.prevent="editCategory" class="form" method="POST" enctype="multipart/form-data">
            <div class="row">     
              <div class="col-lg-12 pt-12">
                <div class="form-group">
                  <label for>Name*</label>
                  <div class="text-danger" v-if="errors.name != undefined" >
                    {{this.errors.name[0]}}
                  </div>
                  <input :class="{'error': errors.name}" type="text" class="form-control" placeholder="Name" v-model="category.name"/>
                </div>
              </div>
              
            </div>
            <div class="form-group mt-2">
            <button v-if="loading" class="btn btn-success">UPDATE</button>
            <button v-else disabled class="btn btn-info"><i class="fas fa-spinner fa-pulse"></i></button>
          </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import { mapState } from "vuex";
export default {
  props: ["identify"],
  data() {
    return {
      loading: true,
      errors: {},
    };
  },
  created() {
    this.$store.dispatch("getCategory", this.identify);
  },
  computed: {
    ...mapState({
      category: (state) => state.categories.category.data,
    }),
  },
  methods: {
    editCategory() {
      this.loading = false;
      this.category.identify = this.identify;
      this.$store
        .dispatch("editCategory", this.category)
        .then((response) => {
          this.$router.push({ name: "categories.index" });
        })
        .catch((error) => {
          this.errors = error.response.data.errors
        })
        .finally(() => (this.loading = true));
    },

  },
}
</script>

<style scoped>
  @import url("https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css");
</style>