<script lang="ts">
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';

  // --- CONTROLE DE CENA ---
  let fase = 0;
  let estiloBalao = 'jose';
  let texto = "";
  let dialogoAberto = false;
  let avatarAtual = 'jose';

  // --- CONTROLE DO MINI-GAME ---
  let miniGameAtivo = false;
  let posicaoMira = 10;
  let direcaoMira = 2;
  let podeAtirar = true;
  let intervaloMira: ReturnType<typeof setInterval> | undefined;
  
  let cabecaEsquerda = 40;
  let cabecaDireita = 60;
  let acertou = false;
  let gameOver = false;

  // --- SEQUÊNCIA DE DIÁLOGOS ---
  const dialogoFinal = [
    { falante: 'jose', texto: 'Parado aí, malfeitor.' },
    { falante: 'marina', texto: 'José, você finalmente chegou! Por favor, me ajude!' },
    { falante: 'vilao', texto: 'Paradinhos... vocês dois aí.A Quanto tempo, José.' },
    { falante: 'jose', texto: 'Estou sem tempo para conversa. Solte ela.' },
    { falante: 'vilao', texto: 'Hahaha... e você vai fazer o quê ein detetive?' },
    { falante: 'jose', texto: 'Eu só preciso de uma bala.' },
    { falante: 'vilao', texto: 'Você não tem peito.' },
    { falante: 'jose', texto: 'Quer apostar?' },
  ];

  // Mapeamento das imagens
  const imagensPersonagens = {
    jose: 'Josédp.png',
    marina: 'marina.png',
    vilao: 'batista.png'
  };

  onMount(() => {
    iniciarCena();
  });

  function iniciarCena() {
    fase = 0;
    dialogoAberto = true;
    miniGameAtivo = false;
    acertou = false;
    gameOver = false;
    mostrarFala();
  }

  function mostrarFala() {
    if (fase < dialogoFinal.length) {
      const falaAtual = dialogoFinal[fase];
      texto = falaAtual.texto;
      estiloBalao = falaAtual.falante;
      avatarAtual = falaAtual.falante; 
      dialogoAberto = true;
    } else {
      dialogoAberto = false;
      iniciarMiniGame();
    }
  }

  function avancarDialogo() {
    if (!dialogoAberto) return;
    fase++;
    mostrarFala();
  }

  // --- MINI-GAME ---
  function iniciarMiniGame() {
    miniGameAtivo = true;
    podeAtirar = true;
    posicaoMira = 10;
    direcaoMira = 2;

    intervaloMira = setInterval(() => {
      posicaoMira += direcaoMira;
      if (posicaoMira >= 90 || posicaoMira <= 10) {
        direcaoMira *= -1;
      }
      posicaoMira = posicaoMira;
    }, 30); // Velocidade da mira
  }

  function atirar() {
    if (!miniGameAtivo || !podeAtirar) return;

    podeAtirar = false;
    clearInterval(intervaloMira);

    if (posicaoMira >= cabecaEsquerda && posicaoMira <= cabecaDireita) {
      acertou = true;
      miniGameAtivo = false;
      dialogoAberto = true;
      estiloBalao = 'jose';
      texto = "O tiro ecoa pelo terraço. O vilão cai. Marina está salva! Você venceu, detetive!";
      avatarAtual = 'jose';
    } else {
      gameOver = true;
      miniGameAtivo = false;
      dialogoAberto = true;
      estiloBalao = 'vilao';
      texto = "Vilão: 'Errou, detetive! A sua hora chegou!' (Fim de jogo)";
      avatarAtual = 'vilao';
    }
  }

  function voltarMenu() {
    window.location.href = "/";
  }
</script>

<style>
  .tela, .cenario {
    position: relative;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    margin: 0;
    padding: 0;
  }

  /* --- FUNDO (você já ajustou, deixei cover) --- */
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
    border-radius: 8px;
    cursor: pointer;
    font-family: 'SpecialElite', serif;
    transition: transform 0.1s;
  }
  .btn-voltar-provisorio:hover { transform: scale(1.05); background-color: #cc0000; }

  /* --- BALÕES E PERSONAGENS --- */
  .container-dialogo {
    position: absolute;
    bottom: 20px;
    left: 20px;
    display: flex;
    align-items: flex-end;
    gap: 0px; 
    z-index: 10;
    width: 90%;
    max-width: 900px;
    pointer-events: none;
  }

  /* Tamanho padrão para todos os personagens */
  .personagem-wrapper { 
    width: 160px;            
    height: 180px;           
    flex-shrink: 0; 
    margin-bottom: -10px; 
    display: flex; 
    align-items: flex-end; 
    justify-content: center;
  }

  .finn-img { 
    width: 100%; 
    height: 100%;            
    object-fit: contain;     
    display: block; 
  }

  .balao-container {
    display: flex;
    flex-direction: column; 
    align-items: flex-start;
    gap: 10px;
    pointer-events: auto;
    margin-left: 5px;
  }

  .balao-css {
    padding: 25px 40px 25px 30px;
    font-family: 'SpecialElite', serif;
    font-size: 1.3rem;
    position: relative;
    min-height: 80px;
    max-width: 450px;
    width: auto;
    line-height: 1.4;
    border-radius: 5px;
  }

  .balao-css::before {
    content: "";
    position: absolute;
    left: -20px;
    bottom: 15px;
    border-width: 10px 20px 10px 0;
    border-style: solid;
  }
  .balao-css::after {
    content: "";
    position: absolute;
    left: -24px;
    bottom: 13px;
    border-width: 12px 20px 12px 0;
    border-style: solid;
    z-index: -1;
  }

  /* José (Papiro) */
  .balao-css.jose {
    background-color: #f1e3c3;
    border: 3px solid #8b6b4d;
    color: #3e2723;
    box-shadow: 0 4px 15px rgba(0,0,0,0.5);
  }
  .balao-css.jose::before { border-color: transparent #f1e3c3 transparent transparent; }
  .balao-css.jose::after { border-color: transparent #8b6b4d transparent transparent; }

  /* Vilão (Preto/Vermelho) */
  .balao-css.vilao {
    background-color: #111111;
    border: 3px solid #b30000;
    color: #e0e0e0;
    box-shadow: 0 4px 15px rgba(255, 0, 0, 0.3);
  }
  .balao-css.vilao::before { border-color: transparent #111111 transparent transparent; }
  .balao-css.vilao::after { border-color: transparent #b30000 transparent transparent; }

  /* Marina (Claro/Suave) */
  .balao-css.marina {
    background-color: #fdf6e3;
    border: 3px solid #ccb699;
    color: #2c1b10;
    box-shadow: 0 4px 15px rgba(255, 255, 255, 0.2);
  }
  .balao-css.marina::before { border-color: transparent #fdf6e3 transparent transparent; }
  .balao-css.marina::after { border-color: transparent #ccb699 transparent transparent; }

  /* --- BOTÃO PROSSEGUIR --- */
  .btn-avancar {
    background-color: #6b4c33;
    color: white;
    border: 2px solid #3e2723;
    padding: 8px 25px;
    font-family: 'SpecialElite', serif;
    font-size: 1.2rem;
    border-radius: 5px;
    cursor: pointer;
    transition: 0.2s;
  }
  .btn-avancar:hover { background-color: #4a3422; transform: scale(1.05); }

  /* --- TELA DO MINI-GAME (MIRA SNIPER) --- */
  .tela-minigame { position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 15; pointer-events: none; }
  
  .mira {
    position: absolute; 
    top: 22%; /* Ajuste a altura aqui */
    width: 50px; 
    height: 50px;
    transform: translateX(-50%);
    pointer-events: none;
    transition: left 0.05s linear; 
    z-index: 16;
    /* Desenho do reticulado feito com CSS puro */
    background: 
      linear-gradient(to right, transparent 45%, rgba(255, 0, 0, 0.9) 45%, rgba(255, 0, 0, 0.9) 55%, transparent 55%),
      linear-gradient(to bottom, transparent 45%, rgba(255, 0, 0, 0.9) 45%, rgba(255, 0, 0, 0.9) 55%, transparent 55%),
      radial-gradient(circle, transparent 40%, rgba(255, 0, 0, 0.3) 40%, rgba(255, 0, 0, 0.3) 41%, transparent 42%);
    border: 1px solid rgba(255, 0, 0, 0.3);
    border-radius: 50%;
  }

  .btn-atirar {
    position: absolute; bottom: 30px; right: 30px;
    background: #cc0000; color: white; border: 4px solid white;
    border-radius: 50%; width: 100px; height: 100px;
    font-family: 'SpecialElite', serif; font-size: 1.8rem; font-weight: bold;
    cursor: pointer; box-shadow: 0 0 20px rgba(255, 0, 0, 0.4);
    pointer-events: auto; z-index: 17; animation: pulsar 1s infinite;
  }
  .btn-atirar:hover { transform: scale(1.1); background: #ff0000; }
  @keyframes pulsar {
    0% { transform: scale(0.95); box-shadow: 0 0 10px rgba(255,0,0,0.3); }
    50% { transform: scale(1.05); box-shadow: 0 0 30px rgba(255,0,0,0.6); }
    100% { transform: scale(0.95); box-shadow: 0 0 10px rgba(255,0,0,0.3); }
  }

  /* ZONA DE ACERTO (Mude a borda para 'red' para ajustar, depois volte para 'transparent') */
  .zona-cabeca-vilao {
    position: absolute;
    top: 195px; left: 710px; width: 110px; height: 120px;
    z-index: 10;
    border: 1px transparent solid; /* Mude para 'red' para ajustar a zona de acerto */
  }
</style>

<div class="tela">
  <div class="cenario">
    
    <!-- FUNDO -->
    <img src="/images/terraco.jpeg" class="fundo" alt="" aria-hidden="true" />

    <!-- ZONA DE ACERTO -->
    <div class="zona-cabeca-vilao"></div>

    <!-- DIÁLOGO -->
    {#if dialogoAberto}
      <div class="container-dialogo">
        
        <!-- TROCA AUTOMÁTICA DO PERSONAGEM -->
        <div class="personagem-wrapper">
          {#if avatarAtual === 'jose'}
            <img src="/images/Josédp.png" alt="José" class="finn-img" />
          {:else if avatarAtual === 'marina'}
            <img src="/images/marina.png" alt="Marina" class="finn-img" />
          {:else if avatarAtual === 'vilao'}
            <img src="/images/batista.png" alt="Batista" class="finn-img" />
          {/if}
        </div>

        <div class="balao-container">
          <div class="balao-css {estiloBalao}">
            <p>{texto}</p>
          </div>
          <button class="btn-avancar" on:click={avancarDialogo}>Prosseguir</button>
        </div>
      </div>
    {/if}

    <!-- MINI-GAME -->
    {#if miniGameAtivo}
      <div class="tela-minigame">
        <div class="mira" style="left: {posicaoMira}%;"></div>
        <button class="btn-atirar" on:click={atirar}>Fogo!</button>
      </div>
    {/if}

    <!-- VITÓRIA -->
    {#if acertou}
      <div style="position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); text-align: center; z-index: 20; background: rgba(0,0,0,0.7); padding: 40px; border-radius: 15px; color: white; font-family: 'SpecialElite', serif;">
        <h1 style="color: gold; font-size: 3rem;">VITÓRIA!</h1>
        <p style="font-size: 1.5rem; margin-bottom: 30px;">O vilão foi derrotado e Marina está salva!</p>
        <button class="btn-voltar-provisorio" style="position: relative; top: auto; left: auto; background: gold; color: black; border-color: black;" on:click={voltarMenu}>Menu Principal</button>
      </div>
    {/if}

    <!-- GAME OVER -->
    {#if gameOver}
      <div style="position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); text-align: center; z-index: 20; background: rgba(139, 0, 0, 0.8); padding: 40px; border-radius: 15px; color: white; font-family: 'SpecialElite', serif; border: 2px solid red;">
        <h1 style="color: #ff0000; font-size: 3rem;">GAME OVER</h1>
        <p style="font-size: 1.5rem; margin-bottom: 30px;">O vilão foi mais rápido...</p>
        <button class="btn-voltar-provisorio" style="position: relative; top: auto; left: auto;" on:click={voltarMenu}>Menu Principal</button>
      </div>
    {/if}

  </div>
</div>