<template>
  <div class="home">
    <!-- if projects are empty it will be false and wont show anything inside it -->
   <div v-if="projects.length">
     <!-- loop over projects with unique keys -->
     <div v-for="project in projects" :key="project.id">
       <SingleProject :project="project"/>
     </div>
   </div>
  </div>
</template>

<script>
import SingleProject from "../components/SingleProject.vue"

export default {
  name: 'Home',
  components: {SingleProject },
  data(){
    return{
      projects: []
    }
  },
  mounted() {
    fetch('http://localhost:3000/projects')
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message))
  }
}
</script>
