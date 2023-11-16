<template>
    <div class="header">
        <h1 class="h1">Pedido no Tablet</h1>
    </div>
    <div class="data">
      <label for="dataInicio">Data de: </label>
      <input type="date" v-model="dataInicio" />
      <label for="dataFim">  Até: </label>
      <input type="date" v-model="dataFim" />
      <q-btn class="btn" @click="buscarPedidos" label="Buscar Pedidos" />
          <div class="lista" v-if="pedidos && !pedidoSelecionado">
            <h2 style="text-align: center;">Lista de Pedidos</h2>
              <table style="width:100%;">
                  <thead class="table">
                      <tr>
                          <th>ID</th>
                          <th>Cliente</th>
                          <th>Data</th>
                      </tr>
                  </thead>
                  <tbody>
                      <tr class="pedidos" v-for="pedido in pedidos" :key="pedido.id" @click="mostrarDetalhes(pedido.id)">
                          <td>{{ pedido.id }}</td>
                          <td>{{ pedido.cliente }}</td>
                          <td>{{ formatarData(pedido.data) }}</td>
                      </tr>
                  </tbody>
              </table>
          </div>
      <div class="detalhes" v-if="pedidoSelecionado">
        <h2>Detalhes do Pedido:  {{ pedidoSelecionado.id }}</h2>
        <table style="width:100%;">
          <thead class="table">
            <tr>
              <th>Pedido</th>
              <th>Nome do Cliente</th>
              <th>Data</th>
              <th>Nome do Vendedor</th>
              <th>Valor Total</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>{{ pedidoSelecionado.id }}</td>
              <td>{{ pedidoSelecionado.cliente }}</td>
              <td>{{ formatarData(pedidoSelecionado.data) }}</td>
              <td>{{ pedidoSelecionado.vendedor }}</td>
              <td>{{ pedidoSelecionado.valor_total }}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="mercadorias" v-if="pedidoSelecionado">
        <table style="width:100%;">
            <thead class="table">
                <tr>
                <th>Id</th>
                <th>Nome Item</th>
                <th>Quantidade</th>
                <th>Preço</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="item in pedidoSelecionado.itens_pedido" :key="item.id">
                <td>{{ item.id }}</td>
                <td>{{ item.nome }}</td>
                <td>{{ item.quantidade }}</td>
                <td>{{ item.preco }}</td>
                </tr>
            </tbody>
        </table>
      </div>
    </div>
</template>
  
<script>
import axios from "axios";
import moment from "moment";

export default {
data() {
    return {
    dataInicio:"",
    dataFim: "",
    pedidos: null,
    pedidoSelecionado: null,
    };
},
methods: {
    async buscarPedidos() {
    try {
        const response = await axios.get(
        `http://localhost:8090/pedidos?dataInicio=${this.dataInicio}&dataFim=${this.dataFim}`
        );
        this.pedidos = response.data;
        this.pedidoSelecionado = null;
    } catch (error) {
        console.error("Erro ao buscar pedidos:", error);
    }
    },
    async mostrarDetalhes(id) {
        try {
        const response = await axios.get(
        `http://localhost:8090/pedidos/${id}`
        );
        this.pedidoSelecionado = response.data;
        console.log(response.data)
        console.log(id)
        }
        catch (error) {
        console.error("Erro ao buscar pedido selecionado:", error);
    }
    },
        formatarData(data){
            return moment(data).format("DD/MM/YYYY")
    },
},
};
</script>  
<style>
    td{
        color:black;
    }
    .pedidos:hover, .pedidos td:hover{
        background:gray;
        color: white;
    }
    .btn{
        color:white;
        background: darkslategray;
        border: none;
        height: 21.5px;
        margin-left: 5px;
    }
    .btn:hover{
        background: gray;
    }
    .header{
        color: white;
        background: darkslategray;
        height: 50px;
    }
    .data{
        margin-top: 40px;
    }
    .h1{
        padding-top: 6px;
    }
    .detalhes{
        padding-left: 80px;
        padding-right: 80px;
    }
    .mercadorias{
        margin-top: 20px;
        margin-left: 80px;
        margin-right: 80px;
    }
    .lista{
        padding-left: 80px;
        padding-right: 80px;
        text-align: left;
    }
    .table {
        border: 1px solid darkslategray;
        height: 21px;
        background: darkslategray;
        color: white;
    }
</style>
<style lang="sass">
table
    background-color: $blue-grey-2
</style>