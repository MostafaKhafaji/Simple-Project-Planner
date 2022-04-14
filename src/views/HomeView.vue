<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div v-if="projects.length">
      <div v-for="project in filterdProjects" :key="project.id">
        <SingleProject
          :project="project"
          @delete="handleDel"
          @complete="handleComplete"
        />
      </div>
    </div>
  </div>
  <div class="default" v-if="projects.length == 0">
    <p>Add Your Projects First To Appear Here!</p>
  </div>
</template>

<script>
// @ is an alias to /src
import SingleProject from "../components/SingleProject.vue";
import FilterNav from "../components/FilterNav.vue";
export default {
  name: "HomeView",
  components: { SingleProject, FilterNav },
  data() {
    return {
      projects: [],
      current: "all",
    };
  },
  computed: {
    filterdProjects() {
      if (this.current === "completed") {
        return this.projects.filter((pro) => pro.complete);
      }
      if (this.current === "ongoing") {
        return this.projects.filter((pro) => !pro.complete);
      }
      return this.projects;
    },
  },

  mounted() {
    fetch("http://localhost:3000/projects/")
      .then((res) => res.json())
      .then((data) => (this.projects = data))
      .catch((err) => console.log(err.message));
  },
  methods: {
    handleDel(id) {
      this.projects = this.projects.filter((project) => {
        return project.id !== id;
      });
    },
    handleComplete(id) {
      let p = this.projects.find((project) => {
        return project.id === id;
      });
      p.complete = !p.complete;
    },
  },
};
</script>

<style>
.default {
  text-align: center;
  margin-top: 100px;
}
.default p {
  font-size: 60px;
  line-height: 100px;
  color: rgb(214, 68, 68);
}
</style>
