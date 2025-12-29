
<template>
  <div :style="{background: currentStyle.background}"
  class="product-container">
  <div v-if="currentStyle.show" class="product-card">
    <div class="img-container">
      <img :src="currentProduct.image" alt="Product Image" />
    </div>
    <div class="desc-container">
      <h3 :style="{color:currentStyle.color}">
        {{ currentProduct.title }}
      </h3>
      <div class="category-rating">
        <span class="category">{{ currentProduct.category }}</span>
  <div class="rating-container">
    <span class="rating">2.9/5</span>
    <div class="rating-circle" :style="{ background: currentStyle.color }"></div>
    <div class="rating-circle" :style="{ background: currentStyle.color }"></div>
    <div class="rating-circle" :style="{ background: currentStyle.color }"></div>
    <div class="rating-circle-border" :style="{ border: currentStyle.border }"></div>
    <div class="rating-circle-border" :style="{ border: currentStyle.border }"></div>
  </div>


      </div>
      <div class="description">
        <p>{{ currentProduct.description }}</p>
      </div>
      <h3 class="price" :style="{color:currentStyle.color}">${{ currentProduct.price }}</h3>
      <div class="button">
        <button :style="{background: currentStyle.color}">Buy Now</button>
        <button @click="nextProduct" :style="{border: currentStyle.border, color:currentStyle.color}">Next Product</button>
      </div>
    </div>
  </div>
  <div v-else class="card-product-unavailable">
    <p>This Product is Unavailabel to show</p>
    <button @click="nextProduct" :style="{border: currentStyle.border}">Next Product</button>
  </div>

  </div>

</template>

<script setup>
  import {ref, computed} from 'vue';
  import { onMounted } from 'vue';

  const products = ref([]);
  const currentIndex = ref(0);
  const isLoading = ref(true);

  const currentProduct = computed(() => products.value[currentIndex.value] || {});
  const currentStyle = computed(() => {
    const category = currentProduct.value.category;
    if(category === "men's clothing"){
      return {
        background : "linear-gradient(to bottom, var(--color-blue-light) 0%, var(--color-blue-light) 60%, var(--color-white) 60%, var(--color-white) 100%)",
        color : "var(--color-primary-men)",
        border: '2px solid var(--color-primary-men)',
        show: true,
      };
    } else if(category === "women's clothing"){
      return {
        background : "linear-gradient(to bottom, var(--color-pink) 0%, var(--color-pink) 60%, var(--color-white) 60%, var(--color-white) 100%)",
        color : "var(--color-primary-women)",
        border : "2px solid var(--color-primary-women)",
        show:true,
    };
    } else{
      return{
        background: "linear-gradient(to bottom, var(--color-unavailable)0%, var(--color-unavailable) 60%, var(--color-white) 60%, var(--color-unavailable) 100%)",
        color: "var(--color-text-grey)",
        border: "2px solid var(--color-text-grey)",
        show: false,
      };
    }
  });

  const nextProduct = () => {
    if (products.value.length > 0) {
      currentIndex.value = (currentIndex.value + 1) % products.value.length
    }
  }

  onMounted(async () => {
    isLoading.value = true;
    try{
      const res = await fetch("https://fakestoreapi.com/products");
      const data = await res.json();
      products.value = data;
    } catch(e){
      console.error("failed to load products", e);
    } finally{
      isLoading.value = false;
    }
  })
</script>