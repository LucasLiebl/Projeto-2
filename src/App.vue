<script setup>
import { ref } from 'vue';

const novoPreco = ref()
const novoItemNome = ref('')
const mostrarADD = ref(true)

const listaProdutos = ref([

{
  id : 1,
  nome : 'tenis',
  preco : 20,
  quantidade:0
},
{
  id : 2,
  nome : 'roupa',
  preco : 20,
  quantidade:0
}
])

function processaItem(e){
  listaProdutos.value.push({
    id: listaProdutos.value.length + 1,
    nome: novoItemNome.value,
    preco: novoPreco.value,
    quantidade: 0
  })
  novoItemNome.value = ''
  novoPreco.value = 0
  mostrarADD.value = true
}
</script>

<template>
<div v-for="produto in listaProdutos" :key="produto.id" class="item">
<div>
   <p> Nome: {{ produto.nome }}</p>
   <p> Preço: {{ produto.preco }}</p>
  <p> Quantidade: {{ produto.quantidade }}</p>
</div>

</div>
<button @click="mostrarADD = !mostrarADD" v-if="mostrarADD">+</button>
<div v-else>
  <form @submit.prevent="processaItem">
    <label for="novoItemNome">Nome:</label>
    <input type="text" v-model="novoItemNome" required>
    <label for="novoPreco"> Preço:</label>
    <input type="number" v-model="novoPreco" required>
    <button type="submit">Enviar</button>
  </form>
</div>


</template>

<style>
.item{
  background-color: rgb(197, 197, 197);
  margin: 10px;
  color: black;
  padding: 10px;
  border-radius: 7px;
}


</style>
