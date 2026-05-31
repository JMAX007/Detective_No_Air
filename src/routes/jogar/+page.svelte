<div class="tela-jogo">

  <h1>Jogo iniciado</h1>

  <a href="/" class="botao-voltar">
    Voltar ao Menu
  </a>

</div>

<style>
  .tela-jogo {
    height: 100vh;
    background: linear-gradient(black, darkred);
    color: white;

    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 30px;
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