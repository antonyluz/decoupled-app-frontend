<template>
  <div class="users">
    <div class="container">
      <section>
        <h5 class="title drac-heading drac-heading-lg drac-text-cyan-green">
          Novo usuário
        </h5>
        <form v-on:submit.prevent="createUser">
          <input
            placeholder="Nome"
            type="text"
            class="drac-input drac-input-cyan drac-text-green drac-m-xs"
            v-model="form.name"
          />
          <input
            placeholder="Email"
            type="text"
            class="drac-input drac-input-cyan drac-text-green drac-m-xs"
            v-model="form.email"
          />
          <button type="submit" class="drac-btn drac-bg-purple-cyan drac-m-xs">
            Cadastrar
          </button>
        </form>
      </section>
      <section>
        <h5 class="title drac-heading drac-heading-lg drac-text-cyan-green">
          Lista de Usuários
        </h5>
        <ul>
          <li v-for="user in users" :key="user.id">
            <p class="drac-text drac-text-cyan-green">{{ user.name }}</p>
            <small class="drac-text small-text">{{ user.email }}</small>
            <a
              class="destroy drac-bg-pink-purple"
              @click="destroyUser(user.id)"
            ></a>
          </li>
        </ul>
      </section>
    </div>
  </div>
</template>

<style scoped>
.container {
  margin: 4rem auto;
  max-width: 500px;
  width: 90%;
  display: grid;
  grid-gap: 2.5rem;
}

.title {
  margin: 0.7rem 0;
}

form {
  display: grid;
  grid-gap: 1rem;
}

input {
  outline: none;
  color: #e1e8ef;
  margin: 0;
}

input::placeholder {
  color: #999fc6;
}

button {
  width: max-content;
  transition: all 0.3s linear;
  cursor: pointer;
  margin: 0;
}

p {
  margin: 0;
}

ul {
  padding: 0;
  margin: 0;
  display: grid;
  grid-gap: 1rem;
}

li {
  background-color: #2b3a4e;
  padding: 1.2rem 1rem;
  border-radius: 1rem;
  position: relative;
  list-style: none;
  color: #8b98a8;
}
.small-text {
  font-size: 12px;
}
.destroy {
  background-color: #d53e6b;
  width: 24px;
  height: 24px;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: all 0.2s linear;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  right: 1.3rem;
}

.destroy:before,
.destroy:after {
  content: "";
  width: 3px;
  height: 13px;
  background-color: #ececf6;
  border-radius: 1rem;
  position: absolute;
  top: 50%;
  left: 50%;
}

.destroy:before {
  transform: translate(-50%, -50%) rotate(45deg);
}

.destroy:after {
  transform: translate(-50%, -50%) rotate(130deg);
}
</style>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "@/utils/axios";

interface User {
  id: string;
  name: string;
  email: string;
}

export default defineComponent({
  components: {},
  data() {
    return {
      users: [] as User[],
      form: {
        name: "",
        email: "",
      },
    };
  },
  created() {
    this.fetchUsers();
  },
  methods: {
    async fetchUsers() {
      try {
        const { data } = await axios.get("/users");
        this.users = data;
      } catch (error) {
        console.warn(error);
      }
    },
    async createUser() {
      try {
        const { data } = await axios.post("/users", this.form);
        this.users.push(data);

        this.form = {
          name: "",
          email: "",
        };
      } catch (error) {
        console.log(error);
      }
    },
    async destroyUser(id: User["id"]) {
      await axios.delete(`/users/${id}`);
      const userIndex = this.users.findIndex((user) => user.id == id);
      this.users.splice(userIndex, 1);
    },
  },
});
</script>
