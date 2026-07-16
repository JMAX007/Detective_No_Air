<script>
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';

  const textoCompleto = `Hoje Você vai se aventurar com a historia de josé

José cresceu nas ruas de Olinda. Sem pai, com uma mãe batalhadora, ele aprendeu cedo que a verdade era a única moeda que valia a pena. De menino de rua a detetive particular, ele construiu uma reputação na cidade por desvendar o que ninguém ousava tocar.

No último ano, ele conheceu Marina. Ela trouxe luz aos becos escuros que ele costumava percorrer. Mas o destino prega peças. Na noite passada, uma carta anônima apareceu debaixo de sua porta:

"José. Você tem até o por do sol. para me encontrar ou Marina vai pagar por isso. Eu estou no último andar. prend-me se for capaz."

Agora, José acorda no seu apartamento. A carta está na mesa. O relógio está correndo. Cada sala, cada puzzle, cada armadilha o levará até o andar mais alto. O que ele vai encontrar lá? A solução para o mistério... ou o fim de sua própria história?`;

  let textoExibido = "";
  let indice = 0;
  let finalizado = false;

  onMount(() => {
    const intervalo = setInterval(() => {
      if (indice < textoCompleto.length) {
        textoExibido += textoCompleto.charAt(indice);
        indice++;
      } else {
        clearInterval(intervalo);
        finalizado = true;
      }
    }, 10); 

    return () => clearInterval(intervalo);
  });

  function irParaOJogo() {
    goto('/jogar'); /* <<< CAMINHO CORRETO */
  }
</script>

<style>
  .tela-intro {
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #1a1a1a; 
  }

  .livro-container {
    position: relative;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .imagem-livro {
    width: 100%;
    height: 100%;
    object-fit: contain; 
    display: block;
  }

  .texto-historia {
    position: absolute;
    top: 44%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 55%;
    max-width: 550px;
    color: #3e2723;
    font-family: 'SpecialElite', serif;
    font-size: 1rem;
    text-align: justify;
    line-height: 1.5;
    margin: 0;
    white-space: pre-wrap;
  }

  .cursor {
    display: inline-block;
    width: 2px;
    height: 1rem;
    background-color: #3e2723;
    margin-left: 2px;
    vertical-align: text-bottom;
    animation: piscar 0.8s infinite;
  }
  @keyframes piscar {
    0%, 100% { opacity: 1; }
    50% { opacity: 0; }
  }

  .btn-prosseguir {
    position: absolute;
    bottom: 8%;
    left: 50%;
    transform: translateX(-50%);
    background-color: #6b4c33;
    color: white;
    border: 2px solid #3e2723;
    padding: 12px 30px;
    font-family: 'SpecialElite', serif;
    font-size: 1.2rem;
    border-radius: 5px;
    cursor: pointer;
    transition: background 0.2s, transform 0.2s;
    opacity: 0; 
    pointer-events: none;
    z-index: 20;
  }

  .btn-prosseguir.aparecer {
    opacity: 1;
    pointer-events: auto;
  }

  .btn-prosseguir:hover {
    background-color: #4a3422;
    transform: translateX(-50%) scale(1.05);
  }

  @media (max-width: 768px) {
    .texto-historia {
      width: 70%;
      font-size: 0.85rem;
      line-height: 1.4;
    }
  }
</style>

<div class="tela-intro">
  <div class="livro-container">
    <img src="/images/livro-aberto.png" alt="Livro de história" class="imagem-livro" />
    
    <p class="texto-historia">
      {textoExibido}
      {#if !finalizado}
        <span class="cursor"></span>
      {/if}
    </p>

    <button class="btn-prosseguir {finalizado ? 'aparecer' : ''}" on:click={irParaOJogo}>
      Prosseguir
    </button>
  </div>
</div>