<script>
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';

  let texto = "";
  let dialogoAberto = false;
  
  let cofreAberto = false;
  let telaCofreAberta = false; 
  let senhaDigitada = "";
  let mensagemCofre = "";

  let pistolaPega = false;

  onMount(() => {
    iniciarSala();
  });

  function iniciarSala() {
    texto = "Uma sala de serviço. nossa aqui ta mas bagunçado que uma loterica em dia de pagamento.vamos ver um armario e um cofre partiu averiguar";
    dialogoAberto = true;
  }

  function fecharDialogo() {
    dialogoAberto = false;
  }

  function abrirCofreGame() {
    telaCofreAberta = true;
    dialogoAberto = false; 
    senhaDigitada = "";
    mensagemCofre = "A campeã amarelinha é a chave... Quantas vezes ela brilhou no mundo?";
  }

  function teclaDigitada(numero) {
    if (senhaDigitada.length < 4) {
      senhaDigitada += numero;
    }
  }

  function apagarSenha() {
    senhaDigitada = senhaDigitada.slice(0, -1);
  }

  function confirmarSenha() {
    if (senhaDigitada === "5" || senhaDigitada === "05" || senhaDigitada === "005") {
      cofreAberto = true;
      telaCofreAberta = false;
      dialogoAberto = true;
      texto = "Com um clique pesado, o cofre enferrujado se abriu! A chave do terraço está em suas mãos.Otimo agora vamos para o grand finale";
    } else {
      mensagemCofre = "Senha incorreta! Reflita sobre o que a amarelinha conquistou.";
      senhaDigitada = "";
    }
  }

  function fecharCofreGame() {
    telaCofreAberta = false;
    dialogoAberto = true;
    texto = "Desisti do cofre... por enquanto.";
  }

  function interagir(objeto) {
    // 1. BOTÃO DE VOLTAR PARA SALA 4
    if (objeto === "voltar-sala-4") {
      goto('/jogar/sala-4'); 
      return; 
    }

    // 2. COFRE
    if (objeto === "cofre") {
      if (cofreAberto) {
        dialogoAberto = true;
        texto = "O cofre já está escancarado e vazio. A chave do terraço está com você.";
      } else {
        abrirCofreGame();
      }
      return;
    }

    // 3. ARMÁRIO DA PISTOLA
    if (objeto === "armario-pistola") {
      dialogoAberto = true;
      if (!pistolaPega) {
        pistolaPega = true;
        texto = "Você revistou o armário e encontrou uma pistola carregada com apenas uma bala, coberta de poeira. uma bala só isso vai ter que servir";
      } else {
        texto = "O armário está vazio.";
      }
      return;
    }

    // 4. QUALQUER OUTRA COISA
    dialogoAberto = true;
    texto = "Não há nada de interessante aqui.";
  }

  function voltarMenu() {
    window.location.href = "/";
  }
</script>

<style>
  /* --- CONFIGURAÇÃO DA TELA --- */
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
    border-radius: 8px;
    cursor: pointer;
    font-family: 'SpecialElite', serif;
    transition: transform 0.1s;
  }
  .btn-voltar-provisorio:hover { transform: scale(1.05); background-color: #cc0000; }

  /* --- PERSONAGEM E BALÃO --- */
  .container-dialogo {
    position: absolute;
    bottom: 20px;
    left: 20px;
    display: flex;
    align-items: flex-end;
    gap: 5px;
    z-index: 10;
    width: 90%;
    max-width: 900px;
    pointer-events: none;
  }

  .personagem-wrapper { width: 220px; flex-shrink: 0; margin-bottom: -10px; }
  .finn-img { width: 100%; height: auto; display: block; }

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
  }
  .btn-fechar:hover { background: #000; transform: scale(1.1); }

  /* --- COFRE --- */
  .tela-cofre {
    position: absolute;
    top: 0; left: 0; width: 100%; height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: rgba(0, 0, 0, 0.85);
    z-index: 20;
  }

  .cofre-ui {
    background: #343840;
    padding: 40px;
    border-radius: 20px;
    border: 6px solid #8a929c;
    display: flex;
    flex-direction: column;
    align-items: center;
    max-width: 420px;
    width: 90%;
    box-shadow: inset 0 0 40px rgba(0,0,0,0.9), 0 0 50px rgba(0,0,0,0.9);
    font-family: 'SpecialElite', serif;
    color: #dac29c;
  }

  .cofre-ui h3 {
    margin-top: 0;
    font-size: 1.8rem;
    text-align: center;
    letter-spacing: 2px;
    text-shadow: 2px 2px 0px #1a0f0a;
    color: #f0f4f8;
    border-bottom: 3px solid #8a929c;
    padding-bottom: 12px;
    width: 100%;
  }

  .cofre-ui p {
    text-align: center;
    font-size: 1.2rem;
    margin: 15px 0 25px 0;
    line-height: 1.5;
    text-shadow: 1px 1px 0px #1a0f0a;
    color: #c8cdd3;
  }

  .display-senha {
    background: #1a1c21;
    color: #a0e0a0;
    font-family: 'SpecialElite', serif;
    font-size: 2.8rem;
    padding: 10px 20px;
    border-radius: 0px;
    width: 160px;
    text-align: center;
    margin-bottom: 25px;
    border: 4px solid #4a5058;
    box-shadow: inset 0 0 20px rgba(0,0,0,0.9);
    letter-spacing: 8px;
    text-shadow: 0 0 5px rgba(255, 180, 50, 0.3);
  }

  .teclado-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
    width: 100%;
    margin-bottom: 25px;
  }

  .tecla {
    background: #4e555e;
    color: #f0f4f8;
    font-family: 'SpecialElite', serif;
    font-size: 1.8rem;
    padding: 15px 0;
    border-top: 2px solid #7b828b;
    border-bottom: 5px solid #2c3036;
    border-left: 2px solid #5b626b;
    border-right: 2px solid #5b626b;
    border-radius: 6px;
    cursor: pointer;
    box-shadow: 0 4px 6px rgba(0,0,0,0.8);
    transition: all 0.1s;
  }
  .tecla:hover { background: #5e6570; }
  .tecla:active { 
    transform: scale(0.92); 
    border-bottom: 2px solid #140d08;
    border-top: 5px solid #140d08;
    box-shadow: inset 0 2px 5px rgba(0,0,0,0.9);
  }

  .tecla-backspace { background: #3a3f45; border-top-color: #6c737c; border-bottom-color: #23262a; }
  .tecla-backspace:hover { background: #4a4f55; }

  .acoes-cofre {
    display: flex;
    justify-content: space-between;
    width: 100%;
    gap: 15px;
  }

  .btn-cofre {
    padding: 12px 20px;
    border-top: 2px solid #6c737c;
    border-bottom: 4px solid #23262a;
    border-left: 2px solid #4b5158;
    border-right: 2px solid #4b5158;
    background: #3e444a;
    color: #f0f4f8;
    border-radius: 6px;
    cursor: pointer;
    font-family: 'SpecialElite', serif;
    font-size: 1.2rem;
    flex: 1;
    transition: 0.2s;
    box-shadow: 0 2px 5px rgba(0,0,0,0.8);
  }
  .btn-cofre:hover { background: #4a4f55; transform: scale(1.02); }
  
  .btn-cofre-confirmar { border-top-color: #8a939e; border-bottom-color: #28303a; background: #4d5660; }
  .btn-cofre-confirmar:hover { background: #5d6772; }
  
  .btn-cofre-fechar { border-top-color: #79828c; border-bottom-color: #232930; background: #414850; }
  .btn-cofre-fechar:hover { background: #515963; }

  /* --- HOTSPOTS INVISÍVEIS --- */
  .hotspot {
    position: absolute;
    background: transparent;
    border: none;
    cursor: pointer;
    z-index: 5;
    transition: transform 0.2s ease, box-shadow 0.2s ease, background 0.2s ease;
  }

  .hotspot:hover {
    background: rgba(255, 240, 200, 0.15);
    border: 1px solid rgba(255, 240, 200, 0.5);
    transform: scale(1.05);
    box-shadow: 0 0 15px 5px rgba(255, 240, 200, 0.2);
  }

  /* ==================================================== */
  /* ⚠️ ALTERE AQUI AS COORDENADAS DO BOTÃO DE VOLTAR ⚠️ */
  /* ==================================================== */
  
  /* Se o botão não estiver em cima da porta (como na sua foto), 
     mexa no top, left, width e height até acertar */
  .hotspot.voltar-sala-4    { top: 30px; left: 600px; width: 200px; height: 600px; }
  
  .hotspot.cofre           { top: 360px; left: 265px; width: 130px; height: 130px; }
  .hotspot.armario-pistola { top: 80px; left: 950px; width: 300px; height: 550px; }
</style>

<div class="tela">
  <div class="cenario">
    
    <img src="/images/servico.jpeg" class="fundo" />

    <!-- BOTÕES INVISÍVEIS -->
    <div class="hotspot voltar-sala-4" on:click={() => interagir('voltar-sala-4')}></div>
    <div class="hotspot cofre" on:click={() => interagir('cofre')}></div>
    <div class="hotspot armario-pistola" on:click={() => interagir('armario-pistola')}></div>

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

    {#if telaCofreAberta}
      <div class="tela-cofre">
        <div class="cofre-ui">
          <h3>Cofre Antigo</h3>
          <p>{mensagemCofre}</p>
          <div class="display-senha">{senhaDigitada || '_'}</div>
          <div class="teclado-grid">
            <button class="tecla" on:click={() => teclaDigitada('1')}>1</button>
            <button class="tecla" on:click={() => teclaDigitada('2')}>2</button>
            <button class="tecla" on:click={() => teclaDigitada('3')}>3</button>
            <button class="tecla" on:click={() => teclaDigitada('4')}>4</button>
            <button class="tecla" on:click={() => teclaDigitada('5')}>5</button>
            <button class="tecla" on:click={() => teclaDigitada('6')}>6</button>
            <button class="tecla" on:click={() => teclaDigitada('7')}>7</button>
            <button class="tecla" on:click={() => teclaDigitada('8')}>8</button>
            <button class="tecla" on:click={() => teclaDigitada('9')}>9</button>
            <button class="tecla" on:click={() => teclaDigitada('')} style="visibility: hidden;"></button>
            <button class="tecla" on:click={() => teclaDigitada('0')}>0</button>
            <button class="tecla tecla-backspace" on:click={apagarSenha}>&#9003;</button>
          </div>
          <div class="acoes-cofre">
            <button class="btn-cofre btn-cofre-fechar" on:click={fecharCofreGame}>Sair</button>
            <button class="btn-cofre btn-cofre-confirmar" on:click={confirmarSenha}>Confirmar</button>
          </div>
        </div>
      </div>
    {/if}
  </div>
</div>