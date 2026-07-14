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
      texto = "Uma foto antiga de alguém... talvez a mãe do detetive.";
    } 
    else if (objeto === "carta") {
      texto = "Essa carta parece estar lacrada com cera...";
    } 
    else if (objeto === "quadro") {
      texto = "Vários rostos marcados no mapa... o caso 27 parece complicado.";
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
    
    <!-- BOTÃO DE VOLTAR -->
    <button class="btn-voltar-provisorio" on:click={voltarMenu}>
      &larr; Menu Principal
    </button>

    <!-- FUNDO DO QUARTO -->
    <img src="/images/quarto.jpeg" class="fundo" />

    <!-- BOTÕES INVISÍVEIS DO CENÁRIO -->
    <div class="hotspot maçaneta" on:click={() => interagir('maçaneta')}></div>
    <div class="hotspot despertador" on:click={() => interagir('despertador')}></div>
    <div class="hotspot retrato-botao" on:click={() => interagir('retrato')}></div>
    <div class="hotspot carta-botao" on:click={() => interagir('carta')}></div>
    <div class="hotspot quadro-botao" on:click={() => interagir('quadro')}></div>

    <!-- BALÃO E PERSONAGEM -->
    {#if dialogoAberto}
      <div class="container-dialogo">
        
        <!-- CAIXA DE ESCALA (Onde você vai encolher o balão e o personagem) -->
        <div class="escala-ui">
          
          <div class="personagem-wrapper">
            <img src="/images/Josédp.png" alt="José" class="finn-img" />
          </div>
          
          <div class="balao-wrapper">
             <img src="/images/balaojosevdd.png" class="balao-imagem" />
             <p class="texto-balao">{texto}</p>
             <button class="btn-fechar" on:click={fecharDialogo}>X</button>
          </div>

        </div> <!-- FIM DO ESCALA-UI -->

      </div> <!-- FIM DO CONTAINER-DIALOGO -->
    {/if} <!-- FIM DO IF DO BALÃO -->

  </div> <!-- FIM DO CENARIO -->
</div> <!-- FIM DA TELA -->