<template>
  <div>
    <h1 class="centralizado">{{ titulo }}</h1>

    <input type="search" class="filtro" v-on:input="filtro = $event.target.value" placeholder="Filtre pelo título">
    
    <ul class="lista-fotos">
      <li class="lista-fotos-item" v-for="foto of fotosComFiltro">
        <meu-painel :titulo="foto.titulo">
          <img class="imagem-responsiva" :src="foto.url" :alt="foto.titulo">
          <meu-botao rotulo="remover" tipo="button" @botaoAtivado="remove(foto)"  :confirmacao="true" estilo="perigo"/>
        </meu-painel>
      </li>
    </ul>
  </div>
</template>

<script>
  import Painel from '../shared/painel/Painel.vue';
  import Botao from '../shared/botao/Botao.vue';
  
  export default {

    methods: {
      remove() {
        alert(".....");
      }
    },

    components: {
      'meu-painel' : Painel,
      'meu-botao' : Botao
    },
    data() {
      return {
        titulo: "Alurapic",
        fotos: [],
        filtro: ''
      };
    },
    created() {
      let promise = this.$http
        .get("http://localhost:3000/v1/fotos")
        .then(res => res.json())
        .then(fotos => (this.fotos = fotos), ex => console.log("Erro interno"));
    },

    computed: {
      fotosComFiltro(){
        if(this.filtro){
          let reg = new RegExp(this.filtro.trim(),'i');
          return this.fotos.filter(foto => reg.test(foto.titulo));
        }
        else{
          return this.fotos;
        }
      }
    },
  };
</script>

<style>


.centralizado {
  text-align: center;
}

.lista-fotos {
  list-style: none;
}
.lista-fotos-item {
  display: inline-block;
}

.imagem-responsiva {
    width: 100%;
  }
.filtro {
  display: block;
  width: 100%;
}
</style>
