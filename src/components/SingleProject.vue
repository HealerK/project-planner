<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <h3 @click="showDetail = !showDetail">{{ project.title }}</h3>
      <div class="icons">
        <i @click="completeProject" class="fa-solid fa-check tick"></i>
        <router-link :to="{name: 'EditProject',  params:{id: project.id} }">
          <i class="fa-solid fa-pen-to-square"></i>
        </router-link>
        <i @click="deleteProject" class="fa-solid fa-trash"></i>
      </div>
    </div>
    <div v-if="showDetail" class="details">
      <p>{{ project.detail }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "SingleProject",
  props: ["project"],
  data() {
    return {
      showDetail: false,
      uri: "http://localhost:3000/projects/" + this.project.id,
    };
  },
  methods: {
    deleteProject() {
      fetch(this.uri, { method: "DELETE" })
        .then(() => this.$emit("delete", this.project.id))
        .catch((err) => console.log(err.message));
    },
    completeProject() {
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
  background: white;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
  border-left: 4px solid #e90074;
}
h3 {
  cursor: pointer;
}
.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.fa-solid {
  font-size: 24px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
  margin-right: 10px;
}
.fa-solid:hover {
  color: #777;
}

.project.complete {
  border-left: 4px solid #00e908;
}

.project.complete .tick {
  color: #00e908;
}
</style>