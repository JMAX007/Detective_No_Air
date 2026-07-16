<script>
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';

  let texto = "";
  let dialogoAberto = false;

  onMount(() => {
    iniciarDia();
  });

  function iniciarDia() {
    texto = "Ah... acordei. Hoje vai ser um longo dia.";
    dialogoAberto = true;
  }

  function fecharDialogo() {
    dialogoAberto = false;
  }

  function interagir(objeto) {
    if (objeto === "maçaneta") {
      goto('/jogar/sala-2');
      return; 
    }

    dialogoAberto = true;

    if (objeto === "despertador") {
      texto = "Desliguei o despertador. Hora de levantar!";
    } 
    else if (objeto === "retrato") {
      texto = "ai ai a foto de Marina sempre me motivando pela manhã.";
    } 
    else if (objeto === "carta") {
      texto = "ué uma carta vamos ver o que ela diz: 'José aqui é o... você não precisa saber o meu nome só precisa saber que eu vou me vingar pela prisão do meu irmão. Você tem até o por do sol para me encontrar ou sua amada vai pagar por isso. Eu estou no último andar. prenda-me se for capaz. ok preciso correr'";
    } 
    else if (objeto === "quadro") {
      texto = "Vários rostos marcados no mapa... o caso do parque foi complicado mas ja esta resolvido.";
    } 
    else {
      texto = "Apenas um objeto comum nessa sala.";
    }
  }

  function voltarMenu() {
    window.location.href = "/";
  }
</script>

<style src="../../styles/jogar.css"></style>

<div class="tela">
  <div class="cenario">
    
   
    <!-- FUNDO DO QUARTO -->
    <img src="/images/quarto.jpeg" class="fundo" />

    <!-- BOTÕES INVISÍVEIS DO CENÁRIO -->
    <div class="hotspot maçaneta" on:click={() => interagir('maçaneta')}></div>
    <div class="hotspot despertador" on:click={() => interagir('despertador')}></div>
    <div class="hotspot retrato-botao" on:click={() => interagir('retrato')}></div>
    <div class="hotspot carta-botao" on:click={() => interagir('carta')}></div>
    <div class="hotspot quadro-botao" on:click={() => interagir('quadro')}></div>

    <!-- PERSONAGEM E BALÃO CSS -->
    {#if dialogoAberto}
      <div class="container-dialogo">
        
        <!-- Personagem (Aumente aqui o personagem) -->
        <div class="personagem-wrapper">
          <img src="/images/Josédp.png" alt="José" class="finn-img" />
        </div>
        
        <!-- Balão Feito Inteiramente no CSS -->
        <div class="balao-css">
          <p>{texto}</p>
          <button class="btn-fechar" on:click={fecharDialogo}>X</button>
        </div>

      </div>
    {/if}

  </div>
</div>