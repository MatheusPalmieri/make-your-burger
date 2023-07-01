<template>
  <Message :message="message" v-show="message" />

  <div id="table">
    <div>
      <div id="table-heading">
        <div class="order-id">#:</div>
        <div>Clientes:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>

    <div id="table-rows">
      <div class="table-row" v-for="burger in burgers" :key="burger.id">
        <div class="order-number">{{ burger.id }}</div>
        <div>{{ burger.name }}</div>
        <div>{{ burger.bread }}</div>
        <div>{{ burger.meat }}</div>

        <div>
          <ul>
            <li v-for="(opcional, index) in burger.options" :key="index">
              {{ opcional }}
            </li>
          </ul>
        </div>

        <div>
          <select
            name="status"
            class="status"
            @change="updateBurger($event, burger.id)"
          >
            <option value="">Selecione</option>
            <option
              v-for="statusItem in status"
              :key="statusItem.id"
              :value="statusItem.tipo"
              :selected="burger.status == statusItem.tipo"
            >
              {{ statusItem.tipo }}
            </option>
          </select>

          <button class="delete-button" @click="deleteBurger(burger.id)">
            Cancelar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";

export default {
  name: "Dashboard",
  components: {
    Message,
  },
  data() {
    return {
      burgers: null,
      burger_id: null,
      status: [],
      message: null,
    };
  },
  methods: {
    async getRequests() {
      const req = await fetch("http://localhost:3000/burgers");
      const data = await req.json();

      this.burgers = data;

      this.getStatus();
    },

    async getStatus() {
      const req = await fetch("http://localhost:3000/status");
      const data = await req.json();

      this.status = data;
    },

    async deleteBurger(id) {
      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "DELETE",
      });
      const res = await req.json();

      this.getRequests();

      this.message = `Pedido removido com sucesso!`;

      setTimeout(() => (this.message = ""), 2000);
    },

    async updateBurger(event, id) {
      const option = event.target.value;

      const dataJson = JSON.stringify({ status: option });

      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();

      this.message = `Pedido Nº ${res.id} foi atualizado para ${res.status}!`;

      setTimeout(() => (this.message = ""), 2000);
    },
  },
  mounted() {
    this.getRequests();
  },
};
</script>

<style scoped>
#table {
  max-width: 1200px;
  margin: 0 auto;
}

#table-heading,
#table-rows,
.table-row {
  display: flex;
  flex-wrap: wrap;
}

#table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}

.table-row {
  width: 100%;
  padding: 12px;
  border-bottom: 1px solid #ccc;
}

#table-heading div,
.table-row div {
  width: 19%;
}

#table-heading .order-id,
.table-row .order-number {
  width: 5%;
}

select {
  padding: 12px 6px;
  margin-right: 12px;
}

.delete-button {
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

.delete-button:hover {
  background-color: transparent;
  color: #222;
}
</style>
