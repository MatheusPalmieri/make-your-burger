<template>
  <div>
    <Message :message="message" v-show="message" />

    <div>
      <form id="form" @submit="createBurger">
        <div class="input">
          <label for="name">Nome</label>

          <input
            type="text"
            name="name"
            id="name"
            v-model="name"
            placeholder="Digite o seu nome"
          />
        </div>

        <div class="input">
          <label for="bread">Escolha o pão:</label>

          <select name="bread" id="bread" v-model="bread">
            <option value="">Selecione seu pão</option>
            <option v-for="bread in breads" :key="bread.id" :value="bread.tipo">
              {{ bread.tipo }}
            </option>
          </select>
        </div>

        <div class="input">
          <label for="meat">Escolha a carne do seu Burger:</label>

          <select name="meat" id="meat" v-model="meat">
            <option value="">Selecione seu pão</option>
            <option v-for="meat in meats" :key="meat.id" :value="meat.tipo">
              {{ meat.tipo }}
            </option>
          </select>
        </div>

        <div id="options" class="input">
          <label for="options" id="options-title"
            >Selecione os opcionais:</label
          >

          <div
            class="checkbox"
            v-for="optionalItem in optionsData"
            :key="optionalItem.id"
          >
            <input
              type="checkbox"
              name="optional"
              id="optional"
              v-model="options"
              :value="optionalItem.tipo"
            />
            <span>{{ optionalItem.tipo }}</span>
          </div>
        </div>

        <div class="input">
          <input type="submit" class="button" value="Criar Burger" />
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";

export default {
  name: "BurgerForm",
  components: {
    Message,
  },
  data() {
    return {
      name: null,
      meat: null,
      meats: null,
      bread: null,
      breads: null,
      optionsData: null,
      options: [],
      message: null,
    };
  },
  methods: {
    async getIngredients() {
      const req = await fetch("http://localhost:3000/ingredientes");
      const data = await req.json();

      this.meats = data.carnes;
      this.breads = data.paes;
      this.optionsData = data.opcionais;
    },

    async createBurger(event) {
      event.preventDefault();

      const data = {
        name: this.name,
        meat: this.meat,
        bread: this.bread,
        options: Array.from(this.options),
        status: "Solicitado"
      };

      const dataJson = JSON.stringify(data);
      const req = await fetch("http://localhost:3000/burgers", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();

      this.message = `Pedido Nº ${res.id} realizado com sucesso!`;

      setTimeout(() => (this.message = ""), 4000);

      this.name = "";
      this.meat = "";
      this.bread = "";
      this.options = "";
    },
  },
  mounted() {
    this.getIngredients();
  },
};
</script>

<style scoped>
#form {
  max-width: 400px;
  margin: 0 auto;
}

.input {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}

input,
select {
  padding: 5px 10px;
  width: 300px;
}

#options {
  flex-direction: row;
  flex-wrap: wrap;
}

#options-title {
  width: 100%;
}

.checkbox {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}

.checkbox span,
.checkbox input {
  width: auto;
}

.checkbox span {
  margin-left: 6px;
  font-weight: bold;
}

.button {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.button:hover {
  background-color: transparent;
  color: #222;
}
</style>
