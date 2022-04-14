<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="action">
      <h3 @click="showDet">{{ project.title }}</h3>
      <div class="icons">
        <router-link :to="{ name: 'EditView', params: { id: project.id } }">
          <span class="material-icons"> edit </span>
        </router-link>
        <span @click="deleteProject" class="material-icons delete">
          delete
        </span>
        <span @click="toggleComplete" class="material-icons tick"> done </span>
      </div>
    </div>
    <transition name="fade" v-show="details">
      <div class="details">
        <p>{{ project.details }}</p>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      details: false,
      uri: "http://localhost:3000/projects/" + this.project.id,
    };
  },
  methods: {
    showDet() {
      this.details = !this.details;
    },
    deleteProject() {
      fetch(this.uri, { method: "DELETE" })
        .then(() => this.$emit("delete", this.project.id))
        .catch((err) => console.log(err));
    },
    toggleComplete() {
      fetch(this.uri, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ complete: !this.project.complete }),
      })
        .then(() => {
          this.$emit("complete", this.project.id);
        })
        .catch((err) => console.log(err));
    },
  },
};
</script>

<style>
.project {
  margin: 20px auto;
  background-color: white;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.5);
  border-left: 4px solid #e90074;
}
h3 {
  cursor: pointer;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

.action {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.action .material-icons {
  cursor: pointer;
  font-size: 24px;
  color: #bbb;
  margin-left: 10px;
  transition: all 0.3s ease;
}
.action .material-icons:hover {
  color: #777;
}

.complete {
  border-left: 4px solid #00ce89;
}
.complete .tick {
  color: #00ce89;
}

.action .delete:hover {
  color: #e43c3c;
}
</style>
