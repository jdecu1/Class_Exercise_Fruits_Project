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
      products: [
        // Sample dataset to display
        {
          id: 1,
          name: "Apple",
          price: 1.25,
          image:
            "https://upload.wikimedia.org/wikipedia/commons/1/15/Red_Apple.jpg",
        },
        {
          id: 2,
          name: "Banana",
          price: 0.85,
          image:
            "https://upload.wikimedia.org/wikipedia/commons/8/8a/Banana-Single.jpg",
        },
        {
          id: 3,
          name: "Orange",
          price: 1.1,
          image:
            "https://upload.wikimedia.org/wikipedia/commons/c/c4/Orange-Fruit-Pieces.jpg",
        },
      ],
    };
  },

  // ----------------------------
  // Computed Properties
  // ----------------------------
  // Computed properties are derived from reactive data.
  // They automatically re-evaluate when their dependencies change.
  computed: {
    filteredProducts() {
      // Convert search text to lowercase for case-insensitive filtering
      const q = this.searchText.trim().toLowerCase();

      // If input is empty, return the full product list
      if (!q) return this.products;

      // Otherwise, filter products by matching text
      return this.products.filter((p) => p.name.toLowerCase().includes(q));
    },
  },

  // ----------------------------
  // Methods (Event Handlers & Helpers)
  // ----------------------------
  methods: {
    // Called when a product is clicked
    handleSelect(id) {
      // Toggle selection — clicking the same product again deselects it
      this.selectedId = this.selectedId === id ? null : id;
    },

    // Helper function: formats a number as a price with 2 decimals
    formatPrice(n) {
      return Number(n).toFixed(2);
    },
  },

  // ----------------------------
  // Lifecycle Hook
  // ----------------------------
  // created() runs once after component instance is created
  created() {
    console.log("App created — products loaded:", this.products.length);
  },
};
</script>

<!-- ======================================================
     STYLES
     ======================================================
     Vue Single File Components (SFCs) can have both global
     and scoped styles.
     - Global styles: affect the entire app
     - Scoped styles: apply only to this component
-->

<!-- Global styles (CSS variables, color scheme) -->
<style>
:root {
  --bg: #ffffff;
  --text: #0f172a;
  --muted: #64748b;
  --border: #e2e8f0;
  --card-bg: #ffffff;
  --focus: #2563eb;
}
</style>

<!-- Scoped styles (only affect this component) -->
<style scoped>
/* Outer card container */
.card {
  background: var(--card-bg);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 16px;
  width: min(560px, 95vw);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.06);
  color: var(--text);
}

/* Typography styles */
h2 {
  margin: 0 0 6px;
}
.muted {
  color: var(--muted);
  margin: 0 0 12px;
}

/* Search input styling */
input[type="text"] {
  width: 100%;
  padding: 10px 12px;
  border-radius: 10px;
  border: 1px solid var(--border);
  background: transparent;
  color: inherit;
  outline: none;
}
input[type="text"]:focus {
  border-color: var(--focus);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--focus) 25%, transparent);
}

/* Product list styling */
.product {
  display: flex;
  gap: 12px;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1px dashed var(--border);
  padding: 10px 0;
  cursor: pointer;
}
.product:last-child {
  border-bottom: 0;
}

/* Layout for image + text */
.left {
  display: grid;
  grid-template-columns: 56px 1fr;
  gap: 12px;
  align-items: center;
}

/* Product images */
img {
  width: 56px;
  height: 56px;
  object-fit: cover;
  border-radius: 10px;
  border: 1px solid var(--border);
}

/* Product name and price */
.name {
  font-weight: 600;
}
.price {
  color: var(--muted);
}

/* Highlight selected product */
.selected {
  background: color-mix(in srgb, var(--focus) 10%, transparent);
  border-radius: 8px;
  padding: 6px;
}
</style>
