<template>
  <div>
    <h1 class="centralizado">{{ titulo }}</h1>

    <input type="search" class="filtro" @input="filtro = $event.target.value" placeholder="filtre por parte do título">

    <ul class="lista-fotos">
      <li class="lista-fotos-item" v-for="foto of fotosComFiltro">

        <meu-painel :titulo="foto.titulo">

          <imagem-responsiva  :url="foto.url" :titulo="foto.titulo" v-meu-transform:scale.animate="1.2"/>
          <meu-botao
          tipo="button"
          rotulo="REMOVER"
          @botaoAtivado="remove(foto)"
          :confirmacao="true"
          estilo="perigo"/>

        </meu-painel>
      </li>
    </ul>
  </div>
</template>

<script>
import Painel from '../shared/painel/Painel.vue';
import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue';
import Botao from '../shared/botao/Botao.vue';
import transform from '../../directives/Transform';

export default {

  components:{
    'meu-painel' : Painel ,
    'imagem-responsiva' : ImagemResponsiva,
    'meu-botao' : Botao
  },

  data() {
    return {
      titulo: "AluraPic",
      fotos: [],
      filtro: ''
    };
  },

  computed: {
    fotosComFiltro(){
      if(this.filtro){
        /*filtrar */
        let exp = new RegExp(this.filtro.trim(),'i');
        return this.fotos.filter(foto => exp.test(foto.titulo));
      }else{
        return this.fotos;
      }
    }

  },

  methods: {

      remove(foto){
           alert('Remover a foto ' + foto.titulo);}
  },
    directives: {
    'meu-transform': transform
  },
  created() {
    let promise = this.$http
      .get("http://localhost:3000/v1/fotos")
      .then(res => res.json())
      .then(fotos => (this.fotos = fotos));
  }
};
</script>

<style>

.centralizado {
  text-align: center;
}

.lista-fotos {
  list-style: none;
}

.lista-fotos .lista-fotos-item {
  display: inline-block;
}


.filtro{
  display: block;
  width: 100%;
}
</style>
