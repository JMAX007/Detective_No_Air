<script>
  // IMPORTAÇÕES NECESSÁRIAS
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation'; // Importado para trocar de tela com a maçaneta!

  // VARIÁVEIS DE ESTADO
  let texto = "";
  let dialogoAberto = false;
  let estiloBalao = "padrao"; // <-- Essa variável vai controlar a cor do balão

  // FUNÇÃO QUE RODA AUTOMATICAMENTE AO ABRIR A TELA
  onMount(() => {
    iniciarDia();
  });

  function iniciarDia() {
    texto = "Ah... acordei. Hoje vai ser um longo dia.";
    dialogoAberto = true;
    estiloBalao = "padrao"; // Balão começa com o estilo padrão
  }

  // FECHA O BALÃO DE FALA
  function fecharDialogo() {
    dialogoAberto = false;
  }

  // LÓGICA DE INTERAÇÃO COM OS OBJETOS
  function interagir(objeto) {
    
    // 1. A MAÇANETA (Não abre balão, vai para outra tela)
    if (objeto === "maçaneta") {
      goto('/jogar/sala-2'); // Vai para a rota que você vai criar depois
      return; 
    }

    // 2. OS OUTROS OBJETOS (Abre o balão e personaliza a cor)
    dialogoAberto = true;

    if (objeto === "despertador") {
      texto = "Desliguei o despertador. Hora de levantar!";
      estiloBalao = "alerta"; // Balão ficará amarelado (manhã)
    } 
    else if (objeto === "retrato") {
      texto = "Uma foto antiga de alguém... talvez a mãe do detetive.";
      estiloBalao = "retrato"; // Balão cor de papel velho (memórias)
    } 
    else if (objeto === "carta") {
      texto = "Essa carta parece estar lacrada com cera...";
      estiloBalao = "carta"; // Balão cor de pergaminho com borda (mistério)
    } 
    else if (objeto === "quadro") {
      texto = "Vários rostos marcados no mapa... o caso 27 parece complicado.";
      estiloBalao = "quadro"; // Balão escuro (suspense)
    } 
    else {
      texto = "Apenas um objeto comum nessa sala.";
      estiloBalao = "padrao";
    }
  }

  // BOTÃO PROVISÓRIO DE VOLTAR AO MENU
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
    <!-- BOTÕES INVISÍVEIS DO CENÁRIO (Hotspots)                     -->
    <!-- ATENÇÃO: Eles estão com fundo vermelho e borda amarela      -->
    <!-- para você conseguir posicionar exatamente em cima dos       -->
    <!-- objetos. Quando acertar, vá no final da mensagem e eu te    -->
    <!-- ensino como esconder eles.                                 -->
    <!-- ========================================================== -->
    
    <!-- Maçaneta -->
    <div class="hotspot" style="top: 290px; left: 630px; width: 80px; height: 180px;" on:click={() => interagir('maçaneta')}></div>
    
    <!-- Despertador -->
    <div class="hotspot" style="top: 210px; left: 365px; width: 60px; height: 50px;" on:click={() => interagir('despertador')}></div>
    
    <!-- Retrato -->
    <div class="hotspot" style="top: 170px; left: 340px; width: 60px; height: 55px;" on:click={() => interagir('retrato')}></div>
    
    <!-- Carta -->
    <div class="hotspot" style="top: 480px; left: 540px; width: 130px; height: 80px;" on:click={() => interagir('carta')}></div>
    
    <!-- Quadro acima da carta -->
    <div class="hotspot" style="top: 120px; left: 610px; width: 170px; height: 330px;" on:click={() => interagir('quadro')}></div>

    <!-- ========================================================== -->
    <!-- BALÃO E IMAGEM DO PERSONAGEM                                -->
    <!-- Repare que a classe do balão agora tem {estiloBalao},       -->
    <!-- isso faz ele trocar de cor de acordo com o botão clicado.   -->
    <!-- ========================================================== -->
    {#if dialogoAberto}
      <div class="container-dialogo">
        <div class="personagem-wrapper">
          <!-- IMAGEM DO FINN -->
          <img src="/images/Finndm.png" alt="Finn" class="finn-img" />
        </div>
        
        <!-- BALÃO DE FALA (Com classe dinâmica {estiloBalao}) -->
        <div class="balao-fala {estiloBalao}">
          <p>{texto}</p>
          <button class="btn-fechar" on:click={fecharDialogo}>X</button>
        </div>
      </div>
    {/if}

  </div>
</div>