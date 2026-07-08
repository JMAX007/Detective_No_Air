<script>
  import { onMount } from 'svelte';

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
    dialogoAberto = true;
    if (objeto === "despertador") {
      texto = "Desliguei o despertador. Hora de levantar!";
    } else if (objeto === "carta") {
      texto = "Essa carta parece estar lacrada com cera...";
    } else {
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
    
    <!-- BOTÃO DE VOLTAR (Provisório) -->
    <button class="btn-voltar-provisorio" on:click={voltarMenu}>
      &larr; Menu Principal
    </button>

    <!-- FUNDO -->
    <img src="/images/quartojogo.jpg" class="fundo" />

    <!-- BOTÕES INTERATIVOS DO CENÁRIO -->
    <div class="hotspot" style="top: 200px; left: 300px; width: 50px; height: 50px;" on:click={() => interagir('despertador')}></div>
    <div class="hotspot" style="top: 450px; left: 100px; width: 180px; height: 120px;" on:click={() => interagir('cama')}></div>
    <div class="hotspot" style="top: 470px; left: 750px; width: 100px; height: 70px;" on:click={() => interagir('carta')}></div>

    <!-- BALÃO E IMAGEM DO PERSONAGEM -->
    {#if dialogoAberto}
      <div class="container-dialogo">
        <div class="personagem-wrapper">
          <!-- AQUI É ONDE A IMAGEM DO PERSONAGEM APARECE -->
          <img src="/images/Finn.png" alt="Finn" class="finn-img" />
        </div>
        <div class="balao-fala">
          <p>{texto}</p>
          <button class="btn-fechar" on:click={fecharDialogo}>X</button>
        </div>
      </div>
    {/if}

  </div>
</div>