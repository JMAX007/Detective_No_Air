<script>
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';

  let texto = "";
  let dialogoAberto = false;
  let estiloBalao = "padrao";

  onMount(() => {
    iniciarDia();
  });

  function iniciarDia() {
    texto = "Ah... acordei. Hoje vai ser um longo dia.";
    dialogoAberto = true;
    estiloBalao = "padrao";
  }

  function fecharDialogo() {
    dialogoAberto = false;
  }

  function interagir(objeto) {
    // MAÇANETA (Vai para a nova tela e não abre balão)
    if (objeto === "maçaneta") {
      goto('/jogar/sala-2');
      return; 
    }

    // ABRE O BALÃO E DEFINE O TEXTO E A COR PARA CADA OBJETO
    dialogoAberto = true;

    if (objeto === "despertador") {
      texto = "Desliguei o despertador. Hora de levantar!";
      estiloBalao = "alerta"; 
    } 
    else if (objeto === "retrato") {
      texto = "Minha paixão sara, um dia irei conquiatala.";
      estiloBalao = "retrato"; 
    } 
    else if (objeto === "carta") {
      texto = "Ué uma carta, nao estava esperando correspondencia vamos ver *Finn não estamos brincando sabemos quem voce é e o que est tramando voce detetive me poupe se quiser sair dessa enrrascada e salvar sua amada, saia daí se for capaz e venha me enfrentar...";
      estiloBalao = "carta"; 
    } 
    else if (objeto === "quadro") {
      texto = "Vários rostos marcados no mapa... o caso 27 parece complicado.";
      estiloBalao = "quadro"; 
    } 
    else {
      texto = "Apenas um objeto comum nessa sala.";
      estiloBalao = "padrao";
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
    <img src="/images/quarto.jpeg" class="fundo" />

    <!-- ========================================================== -->
    <!-- BOTÕES INVISÍVEIS DO CENÁRIO (Com classes no CSS)          -->
    <!-- ========================================================== -->
    
    <div class="hotspot maçaneta" on:click={() => interagir('maçaneta')}></div>
    <div class="hotspot despertador" on:click={() => interagir('despertador')}></div>
    <div class="hotspot retrato-botao" on:click={() => interagir('retrato')}></div>
    <div class="hotspot carta-botao" on:click={() => interagir('carta')}></div>
    <div class="hotspot quadro-botao" on:click={() => interagir('quadro')}></div>

    <!-- ========================================================== -->
    <!-- BALÃO E IMAGEM DO PERSONAGEM                               -->
    <!-- ========================================================== -->
    {#if dialogoAberto}
      <div class="container-dialogo">
        <div class="personagem-wrapper">
          <img src="/images/Finndm.png" alt="Finn" class="finn-img" />
        </div>
        
        <!-- AQUI A MÁGICA DO BALÃO PERSONALIZADO ACONTECE -->
        <div class="balao-fala {estiloBalao}">
          <p>{texto}</p>
          <button class="btn-fechar" on:click={fecharDialogo}>X</button>
        </div>
      </div>
    {/if}

  </div>
</div>