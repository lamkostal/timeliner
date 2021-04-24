<template>
  <header>
      <div class="wrapper">
          <h1>Simple timeliner</h1>
      </div>
  </header>
</template>

<script>
export default {

}
</script>

<style>

@import url('https://fonts.googleapis.com/css2?family=Oi&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Galada&display=swap');
    header{
        position: fixed;
        top:0;
        left:0;
        width: 100%;
        display: grid;
        place-content: center;
        /* height: 50px; */
        background: var(--accent);
        z-index: 100;
    }
    .wrapper h1{
        padding: 0.1em;
        margin: 0;
        font-family: 'Galada', cursive;
        font-weight: 400;
        font-size: 2.9rem;
        line-height: 80px;

    }
</style>