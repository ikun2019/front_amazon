<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium"></div>
            <h2 style="text-align: center;">Update {{ product.title }}</h2>
            <form>
              <!-- Category -->
              <div class="a-spacing-top-medium">
                <label>Category</label>
                <select
                  v-model="categoryID"
                  class="a-select-option"
                >
                  <option
                    v-for="(category, index) in categories"
                    :key="index"
                    :value="category.id"
                  >
                    {{ category.type }}
                  </option>
                </select>
              </div>
              <!-- Owner -->
              <div class="a-spacing-top-medium">
                <label>Owner</label>
                <select
                  v-model="ownerID"
                  class="a-select-option"
                >
                  <option
                    v-for="(owner, index) in owners"
                    :key="index"
                    :value="owner.id">
                    {{ owner.name }}
                  </option>
                </select>
              </div>
              <!-- Title -->
              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px;">Title</label>
                <input
                  v-model="title"
                  type="text"
                  class="a-input-text"
                  style="width: 100%;"
                  :placeholder="product.title"
                >
              </div>
              <!-- Price -->
              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px;">Price</label>
                <input
                  v-model="price"
                  type="number"
                  class="a-input-text"
                  style="width: 100%;"
                  :placeholder="product.price"
                >
              </div>

              <!-- StockQuantity -->
              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px;">Stock Quantity</label>
                <input
                  v-model="stockQuantity"
                  type="number"
                  class="a-input-text"
                  style="width: 100%;"
                  :placeholder="product.stockQuantity"
                >
              </div>

              <!-- Description -->
              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px;">Description</label>
                <textarea
                  v-model="description"
                  style="width: 100%;"
                  :placeholder="product.description"
                ></textarea>
              </div>

              <!-- Photo -->
              <div class="a-spacing-top-medium">
                <label style="margin-bottom: 0px;">Add Photo</label>
                <div class="a-row a-spacing-top-medium">
                  <label class="choosefile-button">
                    <i class="fal fa-plus"></i>
                    <input
                      @change="onFileSelected"
                      type="file"
                    >
                    <p style="margin-top: -70px;">{{ fileName }}</p>
                  </label>
                </div>
              </div>
              <!-- Button -->
              <hr>
              <div class="a-spacing-top-large">
                <span class="a-button-register">
                  <span class="a-button-inner">
                    <span class="a-button-text" @click="onUpdateProduct">Update Product</span>
                  </span>
                </span>
              </div>
            </form>
          </div>
        </div>
        <div class="col-sm-3"></div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  async asyncData({ $axios, params }) {
    try {
      let categories = $axios.$get('http://localhost:8080/api/categories');
      let owners = $axios.$get('http://localhost:8080/api/owners');
      let product = $axios.$get(`http://localhost:8080/api/products/${params.id}`);
  
      const [catResponse, ownerResponse, productResponse] = await Promise.all([
        categories,
        owners,
        product
      ]);
      
      console.log(productResponse);

      return {
        categories: catResponse.categories,
        owners: ownerResponse.owners,
        product: productResponse.product
      }
    } catch (err) {
      console.log(err);
    }
  },
  data() {
    return {
      categoryID: null,
      ownerID: null,
      title: "",
      price: "",
      description: "",
      selectedFile: null,
      fileName: "",
      stockQuantity: ""
    }
  },
  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
      console.log(this.selectedFile);
      this.fileName = event.target.files[0].name;
    },
    async onUpdateProduct() {
      let data = new FormData();
      data.append("title", this.title);
      data.append("price", this.price);
      data.append("description", this.description);
      data.append("ownerID", this.ownerID);
      data.append("categoryID", this.categoryID);
      data.append("photo", this.selectedFile, this.selectedFile.name);
      data.append("stockQuantity", this.stockQuantity);

      let result = await this.$axios.$put(`http://localhost:8080/api/products/${this.$route.params.id}`, data);
      this.$router.push('/');
    }
  }
}
</script>
