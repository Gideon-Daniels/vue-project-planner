<template>
  <h1>Edit project</h1>
  <form @submit.prevent="handleSubmit">
    <label>Title:</label>
    <input type="text" v-model="title" required>

    <label> Details:</label>
    <textarea v-model="details" required ></textarea>

    <button>Update Project</button>
  </form>
</template>

<script>
export default {
    props: ['id'],
    data(){
        return {
            title: '',
            details: '',
            uri: 'http://localhost:3000/projects/'+this.id
        }
    },
    mounted() {
        fetch(this.uri)
        .then( res => res.json())
        .then(data => {
            this.title = data.title
            this.details = data.details
        })
    },
    methods: {
        handleSubmit(){
            let project = {
                title : this.title,
                details : this.details,
            }
            console.log("Working")
        // update 
            fetch(this.uri, {
                method: 'PATCH',
                headers: {'Content-Type': 'application/json'},
                body: JSON.stringify(project)
            })
            // redirect
            .then( ()=> {
                this.$router.push('/')
            })
            .catch( (err) => console.log(err))
        }
    }
}
</script>

<style>

</style>