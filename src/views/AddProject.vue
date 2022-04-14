<template>
  <form @submit.prevent="handleSubmit">
    <div class="box">
      <input
        type="text"
        name="title"
        id="title"
        required
        v-model.trim="title"
      />
      <label for="title">Title:</label>
    </div>
    <label for="details">Details:</label>

    <textarea id="details" v-model.trim="details" required></textarea>

    <button>Add Project</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      title: "",
      details: " ",
    };
  },
  methods: {
    handleSubmit() {
      let project = {
        title: this.title,
        details: this.details,
        complete: false,
      };
      fetch("http://localhost:3000/projects", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(project),
      }).then(() => {
        this.$router.push("/");
      });
    },
  },
};
</script>

<style scoped>
form {
  background: white;
  padding: 20px;
  border-radius: 10px;
}
label {
  display: block;
  color: #bbb;
  text-transform: uppercase;
  font-size: 14px;
  font-weight: bold;
  letter-spacing: 1px;
  margin: 20px 0 10px 0;
}
input {
  padding: 10px;
  border: 0;
  border-bottom: 1px solid #ddd;
  width: 100%;
  box-sizing: border-box;
}
textarea {
  border: 1px solid #ddd;
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
  height: 100px;
}
form button {
  display: block;
  margin: 20px auto 0;
  background: #00ce89;
  color: white;
  padding: 10px;
  border: 0;
  border-radius: 6px;
  font-size: 16px;
  transition: all 0.3s ease;
}
form button:hover,
form button:active {
  background: #03f0a1;
  transform: scale(1.1);
}
.box {
  position: relative;
  margin-bottom: 40px;
  margin-top: 20px;
}
.box label {
  position: absolute;
  top: -10px;
  transition: all 0.3s ease;
}

.box input:valid ~ label,
.box input:focus ~ label {
  top: -45px;
}
</style>
