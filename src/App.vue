<template>
  <div id="app">
    
    <div class="urna">

      <TelaUrna :tela="tela" :numeroVoto="numeroVoto" :quantidadeNumeros="quantidadeNumeros" :candidato="candidato" />
      <TecladoUrna :adicionarNumero="adicionarNumero" :corrigir="corrigir" :confirmar="confirmar"
        :votarEmBranco="votarEmBranco" />


    </div>

  </div>
</template>

<script>

import '@/css/global.css'

import TecladoUrna from './components/TecladoUrna.vue'
import TelaUrna from './components/TelaUrna.vue'

import confirmAudio from './assets/audios/confirm.wav'
import keyAudio from './assets/audios/key.wav'


export default {
  name: 'App',
  components: {
    TecladoUrna,
    TelaUrna
},
methods:{
  adicionarNumero(numero){
    //executa som da tela
    this.executarSom(keyAudio);

    //verifica limite de numero selecionado
    if(this.numeroVoto.length == this.quantidadedeNumeros){
      return false;
    }

    //adicionao numero selecionado
    this.numeroVoto += ''+numero;

    //verifica candidato votado
    this.verificarCandidato();
  },
  verificarCandidato(){
    //voto incompleto
    if(this.numeroVoto.length < this.quantidadeNumeros){
      return false;
    }

    //verifica candidato existente
    if(this.candidatos[this.tela][this.numeroVoto]){
      this.candidato = this.candidatos[this.tela][this.numeroVoto];
      return true;
    }

    //voto nulo
    this.candidato = {
      nome: 'Voto nulo',
      imagem: ''
    }
  },
  corrigir(){
    //executa som da tela
    this.executarSom(keyAudio);

    this.limpar();
  },
  limpar(){
    this.candidato = {}
    this.numeroVoto = ''
  },
  confirmar(){

    if(this.numeroVoto.length < this.quantidadeNumeros){
      return false;
    }

    return this.avancarTela();
  },

  //finalização
  avancarTela(){
    //executa som de confirmação
    this.executarSom(confirmAudio);

    this.tela ='fim';

    //instancia atual
    setTimeout(function(){
      instancia.tela = 'prefeito';
      instancia.quantidadeNumeros = 2;
      return instancia.limpar();
    },3000);
  }, 
  votarEmBranco(){
    if(this.tela == 'fim') return false;

    this.limpar();
    this.avancarTela();
  },
  executarSom(arquivoSom){
    if(arquivoSom){
      let audio = new Audio(arquivoSom);
      audio.play();
    }
  }
},

data(){
  return {
    tela: 'prefeito',
    numeroVoto: '',
    quantidadeNumeros:2,
    candidato:{},
    candidatos: {
      "presidente": {
        "01": {
          "nome": "Ash",
          "partido": "Pokemon",
          "imagem": "https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/ash.png"
        },
        "08": {
          "nome": "Vegeta",
          "partido": "Dragon Ball",
          "imagem": "https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/vegeta.png"
        },
        "09": {
          "nome": "Tails",
          "partido": "Sonic The Hedgehog",
          "imagem": "https://images5.fanpop.com/image/photos/30100000/Sonic-pics-collection-mattthelynx-30145468-208-243.jpg"
        }
      }
      }
    }
  }
}

</script>

<style>
#app {
  background-color: var(--background-color);
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;

}

.urna {
  width: 1000px;
  height: 500px;
  background-color: var(--ballot-box-background-color);
  padding: 30px;
  border-radius: 5px;
  display: flex;
  justify-content: space-between;
}
</style>
