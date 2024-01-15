<template>
  <div class="container" id="app">
    <h1 class="titulo">{{titulo}}</h1>
    <input type="search" class="filtro" v-on:input="filtro = $event.target.value" placeholder="filtre por parte do título">
    <ul class="lista-fotos">
      <li class="lista-fotos-item" v-for="foto of fotosComfiltro">
      <meu-painel :titulo="foto.titulo">
          <imagem-responsive :url="foto.url" :titulo="foto.titulo"/>
      </meu-painel>
      </li>
    </ul>
  </div>
</template>

<script>
  import imgResponsive from '../shared/img-responsive/imgResponsive.vue';
  import painel from '../shared/painel/painel.vue';

export default {

  components: {
    'meu-painel' : painel,
    'imagem-responsive' : imgResponsive
  },
  data() {
    return {
      titulo: "Alura Pic",
      fotos: [
      ],
      filtro: ''
    }
  },

  computed: {
    fotosComfiltro(){
      if(this.filtro){
        let exp = new RegExp(this.filtro.trim(), 'i');
        return this.fotos.filter(foto => exp.test(foto.titulo));
      }else{
        return this.fotos;
      }
    }
  },

  created(){
    let promise = this.$http.get('http://localhost:3000/v1/fotos');
    promise
    .then(res => res.json())
    .then(fotos => this.fotos = fotos);
  }
}
</script>

<style>


  .titulo{
    text-align: center;
  }

  .lista-fotos{
    list-style: none;
  }

  .lista-fotos .lista-fotos-item{
    display: inline-block;
  }



  .filtro{
    display: inline-block;
    width: 100%;

  }
</style>
