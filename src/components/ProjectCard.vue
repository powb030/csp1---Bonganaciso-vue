<template>
  
  <a  :href="project.link || '#'"
    target="_blank"
    rel="noopener noreferrer"
    class="card w-100 h-100 border-0 bg-secondary text-light text-decoration-none project-card-link"
  >
    <img 
      :src="project.image" 
      class="card-img-top" 
      alt="..."
      style="height: 180px; object-fit: cover;"
    />
    <div class="card-body d-flex flex-column">
      <h5 class="card-title">{{ project.title }}</h5>
      <p class="card-text small flex-grow-1">{{ project.description }}</p>
    </div>
  </a>
</template>

<script setup>
import { defineProps } from "vue";
defineProps({
  project: {
    type: Object,
    required: true
  }
});
</script>

<style scoped>
.project-card-link {
  display: flex;
  flex-direction: column;
  border-radius: 8px;
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease, opacity 0.3s ease;
  cursor: pointer;
}

.project-card-link:hover {
  transform: translateY(-8px);
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.4);
  opacity: 0.9;
}
</style>