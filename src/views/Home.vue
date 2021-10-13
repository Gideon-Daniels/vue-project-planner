<template>
  <div class="home">
    <!-- if projects are empty it will be false and wont show anything inside it -->
   <div v-if="projects.length">
     <!-- loop over projects with unique keys -->
     <div v-for="project in projects" :key="project.id">
<SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
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
  created() {
    fetch('http://localhost:3000/projects')
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message))
  },
  // fetch data from db and pass it into projects array
  updated() {
    fetch('http://localhost:3000/projects')
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message))
  },
  // data is being emitted from child component and then is listened to
  methods: {
    handleDelete(id) {
      // filter the projects 
      this.projects = this.projects.filter((project) => {
        return project.id !== id
      })
    },
    handleComplete(id) {
      let p = this.project.find( project => {
        return project.id == id
      })
      p.complete = !p.complete
    }
  }
}
</script>
