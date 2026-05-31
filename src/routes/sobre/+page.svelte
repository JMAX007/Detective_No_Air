<div class="tela-sobre">

  <h1>Sobre o jogo</h1>

  <p class="creators">
    Creators: João Gabriel, João Matheus, Lucas Nunes, Gabriel Mendes
  </p>

  <a href="/" class="botao-voltar">
    Voltar ao Menu
  </a>

</div>

<style>
  .tela-sobre {
    height: 100vh;
    background: #111;
    color: white;

    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 30px;
    text-align: center;
  }

  .creators {
    font-size: 18px;
  }

  .botao-voltar {
    padding: 10px 20px;
    background: #8b4513;
    color: white;
    text-decoration: none;
    border: 2px solid white;
    font-weight: bold;
    transition: 0.2s;
  }

  .botao-voltar:hover {
    background: #a0522d;
  }

  .botao-voltar:active {
    transform: scale(0.95);
  }
</style>