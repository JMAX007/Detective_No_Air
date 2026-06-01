<script>
  let texto = "";

  function clicar(obj) {
    if (obj === "despertador") {
      texto = "Ah... acordei. Hoje vai ser um longo dia.";
    }
  }

  function voltarMenu() {
    window.location.href = "/";
  }
</script>

<style src="../../styles/jogar.css"></style>

<div class="tela">
  <div class="cenario">

    <!-- FUNDO -->
    <img src="/images/quarto.jpeg" class="fundo" />

    <!-- DESPERTADOR -->
    <img 
      src="/images/despertador.png" 
      class="obj_despertador"
      on:click={() => clicar("despertador")}
    />

    <!-- BOTÃO VOLTAR -->
    <button class="btn-voltar" on:click={voltarMenu}>
      Voltar
    </button>

    <!-- CAIXA DE DIÁLOGO -->
    {#if texto}
      <div class="dialogo">
        {texto}
      </div>
    {/if}

  </div>
</div>