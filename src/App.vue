<script setup>
import { ref } from 'vue';

const novoPreco = ref()
const novoItemNome = ref('')
const mostrarADD = ref(true)

const admMode = ref(false)

const listaProdutos = ref([

  {
    id: 1,
    nome: 'tenis',
    preco: 20,
    quantidade: 1
  },
  {
    id: 2,
    nome: 'roupa',
    preco: 20,
    quantidade: 1
  }
])

const carrinho = ref([])

function processaItem(e) {
  listaProdutos.value.push({
    id: listaProdutos.value.length + 1,
    nome: novoItemNome.value,
    preco: novoPreco.value,
    quantidade: 1
  })
  novoItemNome.value = ''
  novoPreco.value = 0
  mostrarADD.value = true
}

function addCarrinho(produto) {
    if (carrinho.value.findIndex(item => item.id == produto.id) < 0){
      const total = produto.quantidade * produto.preco;
      carrinho.value.push({ ...produto, total})
      calculaTotal();
    }

}
function removerLista(produto) {
  listaProdutos.value.splice(produto, 1)
  carrinho.value.splice(produto, 1)
  calculaTotal()
}
function removerCarrinho(produto){
  carrinho.value.splice(produto, 1)
  calculaTotal()
}

function calculaTotal() {
  valorTotal.value = 0
  for(const item of carrinho.value){
    valorTotal.value = valorTotal.value + item.total
  }
}

const valorTotal = ref(0)
</script>

<template>
  <div class="template">
    <h1>Hype Store</h1>
  <div v-if="admMode"  >
    <button @click="admMode = !admMode" class="adm">ADM MODE</button>
    <div class="body">
    <div v-for="produto in listaProdutos" :key="produto.id" class="item">
      <div>
        <p> Nome: {{ produto.nome }}</p>
        <p> Preço: {{ produto.preco }}</p>
        <button @click="removerLista">Remover da Lista</button>
      </div>
    </div>
    </div>

    <button @click="mostrarADD = !mostrarADD" v-if="mostrarADD" id="addLista">Adicionar à lista de Produtos</button>
    <div v-else>

      <form @submit.prevent="processaItem">
        <label for="novoItemNome">Nome:</label>
        <input type="text" v-model="novoItemNome" required>
        <label for="novoPreco"> Preço:</label>
        <input type="number" v-model="novoPreco" required>
        <button type="submit">Enviar</button>
      </form>
    </div>
  </div>
  <div v-else>
    <button @click="admMode = !admMode" class="adm">ADM MODE</button>
    <div class="body">
    <div v-for="produto in listaProdutos" :key="produto.id" class="item">
      <div>
        <p> Nome: {{ produto.nome }}</p>
        <p> Preço: {{ produto.preco }}</p>
        <p> Quantidade: {{ produto.quantidade }}</p>
        <button @click="produto.quantidade++">+</button>
        <button @click="(produto.quantidade > 1) ? produto.quantidade-- : 1">-</button>
        <button @click="addCarrinho(produto)">Adicionar ao carrinho</button>
      </div>
    </div>
    </div>



    <div class="carrinho">
      <h2>carrinho</h2>
      <div class="body">
      <div class="item" v-for="produto in carrinho" :key="produto.id">
        <p> Nome: {{ produto.nome }}</p>
        <p> Preço: {{ produto.preco }}</p>
        <p> Quantidade: {{ produto.quantidade }}</p>
        <p>total: {{ produto.total }}</p>
        <button @click="removerCarrinho">Remover</button>
      </div>
    </div>
    <p>Total: {{ valorTotal }}</p>
    </div>
  </div>
</div>
</template>

<style>
.item {
  background-color: rgb(197, 197, 197);
  margin: 50px;
  color: black;
  padding: 10px;
  border-radius: 7px;
  height: 50%;
  width: 100%;
  border: 2px solid rgb(119, 119, 119);
}
.item:hover{
  background-color: rgb(119, 119, 119);
  transition: 0.35s;
  scale: 1.05;
}
.adm{
  background-color: red;
  border: 2px solid rgb(255, 238, 0);
  color: rgb(255, 238, 0);
  font-weight: bolder;
}
.adm:hover{
  scale: 1.1;
  background-color: rgb(255, 238, 0);
  color: red;
  border: 2px solid red;
  transition: 0.35s;
}
#addLista{
  background-color: rgb(1, 112, 1);
  padding: 5px;
  border: 2px solid black;
}
#addLista:hover{
  scale: 1.1;
  transition: 0.35s;
}
button{
  border-radius: 5px;
  border: 1.5px solid rgb(50, 50, 50);
}
h1{
  font-weight: 900;
  font-style: italic;
}
.body{
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  column-gap: 20px;
}
input{
  border-radius: 5px;
  border: 1.5px solid rgb(50, 50, 50)
}
.template{
  display: grid;
  grid-template-columns: 1fr;
}
</style>
