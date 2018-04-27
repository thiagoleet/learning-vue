

<template>
  <div>
    <h1 class="centralizado">{{ titulo }}</h1>
    <p v-show="mensagem" class="centralizado">{{ mensagem }}</p>
    <input type="search" class="filtro" v-on:input="filtro = $event.target.value" placeholder="filtre pelo título da foto">
    <ul class="lista-fotos">
      <li class="lista-fotos-item" v-for="foto in fotosComFiltro" :key="foto.titulo">
        <meu-painel :titulo="foto.titulo">
          <imagem-responsiva v-meu-transform:scale.animate="1.2" :url="foto.url" :titulo="foto.titulo" />
          <meu-botao rotulo="remover" tipo="button" @botaoAtivado="remove(foto)" :confirmacao="true" estilo="perigo" />
        </meu-painel>
      </li>
    </ul>

  </div>
</template>

<script>
import Painel from "../shared/painel/Painel.vue";
import ImagemResponsiva from "../shared/image-responsiva/ImagemResponsiva";
import Botao from "../shared/botao/Botao";

export default {
  components: {
    "meu-painel": Painel,
    "imagem-responsiva": ImagemResponsiva,
    "meu-botao": Botao
  },
  data() {
    return {
      titulo: "Alurapic",
      fotos: [],
      filtro: "",
      mensagem: ""
    };
  },
  computed: {
    fotosComFiltro() {
      if (this.filtro) {
        /* filtrar */
        let exp = new RegExp(this.filtro.trim(), "i");
        return this.fotos.filter(foto => exp.test(foto.titulo));
      } else {
        /* não filtrar */
        return this.fotos;
      }
    }
  },
  methods: {
    remove(foto) {
      this.resource.delete({ id: foto._id }).then(
        () => {
          let indice = this.fotos.indexOf(foto);
          this.fotos.splice(indice, 1);
          this.mensagem = "Foto removida com sucesso";
        },
        error => {
          console.log(error);
          this.mensagem = "Não foi possível remover a foto";
        }
      );
    }
  },
  created() {
    this.resource = this.$resource("v1/fotos{/id}");
    this.resource
      .query()
      .then(res => res.json())
      .then(fotos => (this.fotos = fotos), err => console.log(err));
  }
};
</script>

<style scoped>
.centralizado {
  text-align: center;
}

.lista-fotos {
  list-style: none;
}

.lista-fotos .lista-fotos-item {
  display: inline-block;
}

.filtro {
  width: 100%;
}
</style>
