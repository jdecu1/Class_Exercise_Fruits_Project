<template>
  <!-- ===============================
       Root Template Section
       ===============================
       Contains HTML markup that Vue will render dynamically.
       Vue directives (v-model, v-for, v-if, :class, @click) are used to
       bind data, render lists, handle events, and reactively update the UI.
  -->
  <div class="card" role="region" aria-labelledby="title">
    <!-- Section title -->
    <h2 id="title">Product List (Options API)</h2>

    <!-- Descriptive text explaining features -->
    <p class="muted">
      Uses <code>data</code>, <code>computed</code>, <code>methods</code>, and
      <code>created</code>.
    </p>

    <!-- v-model provides TWO-WAY DATA BINDING between this input
         and the reactive variable 'searchText' defined in data().
         Any change here updates 'searchText' in real time. -->
    <input
      type="text"
      v-model="searchText"
      placeholder="Search products…"
      aria-label="Search products by name"
    />

    <!-- v-for iterates through the computed property 'filteredProducts'
         to render each product dynamically.
         ':key' improves rendering performance by uniquely identifying items. -->
    <!-- @click triggers a method when a product is clicked -->
    <div
      v-for="p in filteredProducts"
      :key="p.id"
      class="product"
      :class="{ selected: p.id === selectedId }"
      @click="handleSelect(p.id)"
    >
      <div class="left">
        <!-- v-bind (or shorthand ':') dynamically binds attributes like src and alt -->
        <img :src="p.image" :alt="p.name" loading="lazy" />
        <div class="meta">
          <div class="name">{{ p.name }}</div>
          <!-- Method call to format price to 2 decimal places -->
          <div class="price">${{ formatPrice(p.price) }}</div>
        </div>
      </div>
    </div>

    <!-- Conditional rendering:
         Only shown if no products match the search query -->
    <p v-if="filteredProducts.length === 0" class="muted" aria-live="polite">
      No results found.
    </p>
  </div>
</template>

<script>
/* ======================================================
   SCRIPT SECTION (Options API)
   ======================================================
   This part defines the component’s logic — data, computed
   properties, lifecycle hooks, and methods.
*/
export default {
  name: "ProductListOptions",

  // ----------------------------
  // Reactive State
  // ----------------------------
  // The data() function returns an object whose properties
  // become reactive. Vue tracks and updates the DOM automatically
  // when they change.
  data() {
    return {
      searchText: "", // Bound to <input> using v-model
      selectedId: null, // Tracks which product is currently selected
      products: [],
    };
  },

  created() {
    fetch("https://raw.githubusercontent.com/jdecu1/Class_Exercise_Fruits_Project/main/list.json")
      .then(response => response.json())
      .then(data => {
        this.products = data.map((item, index) => ({
          id: index + 1,
          ...item,
          image: this.getImageFor(item.name)
        }));
      })
      .catch(error => console.error("Error loading products:", error));
  },

  // ----------------------------
  // Computed Properties
  // ----------------------------
  // Computed properties are cached based on their dependencies
  // and automatically update when dependencies change.
  computed: {
    filteredProducts() {
      return this.products.filter(p =>
        p.name.toLowerCase().includes(this.searchText.toLowerCase())
      );
    },
  },

  // ----------------------------
  // Methods
  // ----------------------------
  // Define reusable functions that can be invoked by events
  // or inside the template.
  methods: {
    getImageFor(name) {
      const images = {
        Apple: "https://upload.wikimedia.org/wikipedia/commons/1/15/Red_Apple.jpg",
        Banana: "https://upload.wikimedia.org/wikipedia/commons/8/8a/Banana-Single.jpg",
        Orange: "https://upload.wikimedia.org/wikipedia/commons/c/c4/Orange-Fruit-Pieces.jpg",
        Mango: "https://upload.wikimedia.org/wikipedia/commons/9/90/Hapus_Mango.jpg",
        Dragonfruit: "https://upload.wikimedia.org/wikipedia/commons/5/5e/Pitaya_cross_section_ed2.jpg",
        Grapefruit: "https://upload.wikimedia.org/wikipedia/commons/6/6b/Grapefruit_Slice.jpg",
        Cantaloupe: "https://upload.wikimedia.org/wikipedia/commons/f/fd/Cantaloupe_and_cross_section.jpg",
        Plumb: "https://upload.wikimedia.org/wikipedia/commons/f/f1/Plums.jpg",
        Pear: "https://upload.wikimedia.org/wikipedia/commons/3/32/Pear_DS.jpg",
        Peach: "https://upload.wikimedia.org/wikipedia/commons/9/9e/Autumn_Red_peaches.jpg"
      };
      return images[name] || "https://via.placeholder.com/100";
    },

    formatPrice(price) {
      return price.toFixed(2);
    },

    handleSelect(id) {
      this.selectedId = id;
    },
  },
};
</script>

<style scoped>
/* ======================================================
   STYLE SECTION
   ======================================================
   Scoped styles apply only to this component.
*/
.card {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 1rem;
  max-width: 400px;
  margin: 0 auto;
  background: #fff;
}

.muted {
  color: #666;
  font-size: 0.9rem;
}

input {
  width: 100%;
  padding: 0.5rem;
  margin-bottom: 1rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.product {
  display: flex;
  align-items: center;
  padding: 0.5rem;
  border: 1px solid transparent;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.2s;
}

.product:hover {
  background: #f9f9f9;
}

.product.selected {
  border-color: #007bff;
  background: #e9f5ff;
}

.left {
  display: flex;
  align-items: center;
}

.left img {
  width: 50px;
  height: 50px;
  object-fit: cover;
  margin-right: 1rem;
  border-radius: 4px;
}

.meta {
  display: flex;
  flex-direction: column;
}

.name {
  font-weight: bold;
}

.price {
  color: #333;
}
</style>
