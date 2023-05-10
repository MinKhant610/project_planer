<template>
  <div class="home">
    <h1>Home</h1>
    <!-- $event = filterBy function's value  -->
    <FilterNav @filterValue="current = $event" :current="current"></FilterNav>
    <div v-for="project in filterProjects" :key="project.id" >
      <SingleProject :project="project" @delete="deletProject"
      @complete="completeProject"></SingleProject>
    </div>
  </div>
</template>

<script>

import FilterNav from '../components/FilterNav'
import SingleProject from '../components/SingleProject'
export default {
  name: 'HomeView',
  components: {
    FilterNav,
    SingleProject,
    
  },

  data(){
    return {
      projects : [],
      current : "all",
    }
  },

  computed : {
    filterProjects(){
      if (this.current === 'complete'){
        return this.projects.filter(project => project.complete)
      }
      if (this.current === 'ongoing'){
        return this.projects.filter(project => !project.complete)
      }
      return this.projects
    }
 
  },

  mounted(){
    fetch('http://localhost:3000/projects')
    .then((response) => {
      return response.json();
    })
    .then((datas)=>{
      this.projects = datas;
    })
    .catch((err)=>{
      console.log(err)
    })
  },

  methods : {
    // to delete project in UI 
    deletProject(del_id){
      this.projects = this.projects.filter(project=> project.id != del_id);
    },
    completeProject(id){
      let find_project = this.projects.find(project=>project.id === id);
      find_project.complete = !find_project.complete;
    }
  }

}
</script>
