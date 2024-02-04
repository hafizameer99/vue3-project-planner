<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current"/>
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project"  @delete="deleteProject" @complete="toggleComplete"/>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, computed  } from "vue"
import SingleProject from "../components/SingleProject.vue"
import FilterNav from "../components/FilterNav.vue"

  const projects = ref([]);
  const current = ref('all')
  const getProjects = () => {
    fetch('http://localhost:3000/projects')
    .then(res => res.json())
    .then(data => 
      projects.value = data
    )
    .catch(error => console.log(error))
  }
  const deleteProject = (id) => {
    projects.value = projects.value.filter((project) => {
      return project.id !== id
    })
  }
  const toggleComplete = (id) => {
    let p = projects.value.find(project => {
      return project.id === id
    })
    p.complete = !p.complete
  }
  onMounted(() => {
      getProjects()
  });
  const filteredProjects = computed(() => {
     if (current.value === 'completed') {
      return projects.value.filter(project => project.complete);
    } if (current.value === 'ongoing') {
      return projects.value.filter((project) => !project.complete);
    }
    return projects.value
})
</script>
