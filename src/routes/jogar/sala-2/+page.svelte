<script>
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';

  let texto = "";
  let dialogoAberto = false;

  onMount(() => {
    iniciarCorredor();
  });

  function iniciarCorredor() {
    texto = "O corredor está escuro. Não há ninguém, mas ouço barulhos vindo das escadas.";
    dialogoAberto = true;
  }

  function fecharDialogo() {
    dialogoAberto = false;
  }

  function interagir(objeto) {
    // 1. PORTAS DE VOLTA (Voltar para o quarto)
    if (objeto === "porta-voltar") {
      goto('/jogar'); 
      return; 
    }

    // 2. ESCADA (Irá para o próximo andar)
    if (objeto === "escada") {
      goto('/jogar/sala-3');
      return;
    }

    // 3. QUALQUER OUTRA INTERAÇÃO
    dialogoAberto = true;
    texto = "Não há nada de interessante aqui.";
  }

  function voltarMenu() {
    window.location.href = "/";
  }
</script>

<style>
  /* CSS local do Corredor */
  .tela, .cenario {
    position: relative;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    margin: 0;
    padding: 0;
  }

  .fundo {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
    z-index: 1;
  }

  .btn-voltar-provisorio {
    position: absolute;
    top: 20px;
    left: 20px;
    z-index: 9999;
    background-color: #ff4d4d;
    color: white;
    border: 2px solid white;
    padding: 10px 20px;
    font-size: 16px;
    font-weight: bold;
    border-radius: 8px;
    cursor: pointer;
    box-shadow: 0 4px 6px rgba(0,0,0,0.5);
    font-family: 'SpecialElite', serif;
    transition: transform 0.1s, background 0.2s;
  }
  .btn-voltar-provisorio:hover {
    transform: scale(1.05);
    background-color: #cc0000;
  }

  .container-dialogo {
    position: absolute;
    bottom: 20px;
    left: 20px; /* Deixei no canto esquerdo (a opção que você gostou) */
    display: flex;
    align-items: flex-end;
    gap: 5px;
    z-index: 10;
    width: 90%;
    max-width: 900px;
    pointer-events: none;
  }

  .personagem-wrapper {
    width: 220px;
    flex-shrink: 0;
    margin-bottom: -10px;
  }

  .finn-img {
    width: 100%;
    height: auto;
    display: block;
  }

  .balao-css {
    background-color: #f1e3c3;
    border: 3px solid #8b6b4d;
    border-radius: 15px 15px 15px 5px;
    color: #3e2723;
    padding: 30px 50px 30px 30px;
    font-family: 'SpecialElite', serif;
    font-size: 1.5rem;
    position: relative;
    flex-grow: 0;
    box-shadow: 0 4px 15px rgba(0,0,0,0.5);
    line-height: 1.4;
    pointer-events: auto;
    min-height: 100px;
    max-width: 450px;
    width: auto;
  }

  .balao-css::before {
    content: "";
    position: absolute;
    left: -20px;
    bottom: 15px;
    border-width: 10px 20px 10px 0;
    border-style: solid;
    border-color: transparent #f1e3c3 transparent transparent;
  }

  .balao-css::after {
    content: "";
    position: absolute;
    left: -24px;
    bottom: 13px;
    border-width: 12px 20px 12px 0;
    border-style: solid;
    border-color: transparent #8b6b4d transparent transparent;
    z-index: -1;
  }

  .balao-css p {
    margin: 0;
  }

  .btn-fechar {
    position: absolute;
    top: -10px;
    right: -10px;
    background: #333;
    color: #fff;
    border: 2px solid #fff;
    border-radius: 50%;
    width: 32px;
    height: 32px;
    cursor: pointer;
    font-weight: bold;
    font-size: 16px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.5);
  }
  .btn-fechar:hover {
    background: #000;
    transform: scale(1.1);
  }

  /* ========================================================== */
  /* HOTSPOTS DO CORREDOR (Deixe vermelho pra ajustar)          */
  /* ========================================================== */
  .hotspot {
    position: absolute;
    background: transparent;
    border: none;
    cursor: pointer;
    z-index: 5;

    transition: transform 0.2s ease, box-shadow 0.2s ease,background 0.2s ease;
  }
  .hotspot:hover {
  /* Efeito quando o mouse passa por cima */
  background: rgba(255, 240, 200, 0.15); 
  border: 1px solid rgba(255, 240, 200, 0.5);
  transform: scale(1.05); /* Aumenta 5% */
  box-shadow: 0 0 15px 5px rgba(255, 240, 200, 0.2); /* Brilho dourado sutil */
}

  .hotspot.porta-voltar {
    top: 120px; left: 185px; width: 150px; height: 700px; /* Coloque em cima da porta de volta */
  }

  .hotspot.escada { 
    top: 250px; left: 800px; width: 121px; height: 240px; /* Coloque em cima da escada */
  }
</style>

<div class="tela">
  <div class="cenario">
    

    <!-- ADICIONE SUA IMAGEM DO CORREDOR AQUI -->
    <img src="/images/corredor.jpeg" class="fundo" />

    <div class="hotspot porta-voltar" on:click={() => interagir('porta-voltar')}></div>
    <div class="hotspot escada" on:click={() => interagir('escada')}></div>

    {#if dialogoAberto}
      <div class="container-dialogo">
        <div class="personagem-wrapper">
          <img src="/images/Josédp.png" alt="José" class="finn-img" />
        </div>
        <div class="balao-css">
          <p>{texto}</p>
          <button class="btn-fechar" on:click={fecharDialogo}>X</button>
        </div>
      </div>
    {/if}
  </div>
</div>