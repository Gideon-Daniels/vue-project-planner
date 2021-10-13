<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <!-- if projects are empty it will be false and wont show anything inside it -->
    <div v-if="projects.length">
     <!-- loop over projects with unique keys -->
      <div v-for="project in filteredProjects" :key="project.id" v-bind="completed">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from "../components/SingleProject.vue"
import FilterNav from "../components/FilterNav.vue"

export default {
  props: ['current'],
  name: 'Home',
  components: { SingleProject, FilterNav },
  data(){
    return{
      projects: [],
      current: 'all'
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
  },
  computed: {
    filteredProjects() {
      if (this.current === 'completed') {
        return this.projects.filter( project => project.complete)
      }
      if (this.current === 'ongoing') {
        return this.projects.filter( project => !project.complete)
      }
      return this.projects

    }
  }
}
</script>
