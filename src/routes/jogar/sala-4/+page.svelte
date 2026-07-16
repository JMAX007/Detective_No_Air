<script>
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';

  let texto = "";
  let dialogoAberto = false;

  onMount(() => {
    iniciarSala();
  });

  function iniciarSala() {
    texto = "Bom cheguei até aqui vamos ver o que o futuro me proporciona";
    dialogoAberto = true;
  }

  function fecharDialogo() {
    dialogoAberto = false;
  }

  function interagir(objeto) {
    
    // Porta do Terraço (AGORA DESTRANCADA E LEVA PARA O TERRAÇO)
    if (objeto === "porta-terraco") {
      goto('/jogar/terraco');
      return;
    }

    // Porta de Serviços (Leva para a sala de serviço)
    if (objeto === "porta-servico") {
      goto('/jogar/servico');
      return;
    }

    dialogoAberto = true;
    texto = "Não há nada de interessante aqui.";
  }

  function voltarMenu() {
    window.location.href = "/";
  }
</script>

<style>
  /* --- Visual idêntico à sala 3 --- */
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

  
  .hotspot.porta-terraco     { top: 55px; left: 710px; width: 100px; height: 200px; }
  .hotspot.porta-servico    { top: 340px; left: 820px; width: 130px; height: 300px; }
</style> 

<div class="tela">
  <div class="cenario">
    
    
    <img src="/images/sala-3.jpeg" class="fundo" />

    <div class="hotspot voltar-corredor" on:click={() => interagir('voltar-corredor')}></div>
    <div class="hotspot porta-terraco" on:click={() => interagir('porta-terraco')}></div>
    <div class="hotspot porta-servico" on:click={() => interagir('porta-servico')}></div>

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