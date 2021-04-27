<template>
  <header>
      <div class="wrapper">
         <div class="svg_container"> <svg fill-rule="evenodd" stroke-linecap="round" stroke-linejoin="round" stroke-miterlimit="1.5" clip-rule="evenodd" viewBox="0 0 223 61"><defs/><g fill="none" stroke="var(--bg-tl)"><circle cx="12.526" cy="48.358" r="10.56" stroke-width="3.93"/><circle cx="165.04" cy="40.537" r="10.56" stroke-width="3.93"/><circle cx="82.797" cy="12.526" r="10.56" stroke-width="3.93"/><circle cx="209.85" cy="17.446" r="10.56" stroke-width="3.93"/><path stroke-width="3.93" d="M27.704 40.537l39.76-20.488"/><path stroke-width="3.79" d="M180.378 32.634l14.181-7.308"/><path stroke-width="3.97" d="M99.017 18.051l49.807 16.964"/></g></svg></div>
          <h1>Simple timeliner</h1>
      </div>
  </header>
</template>

<script>
export default {

}
</script>

<style>

@import url('https://fonts.googleapis.com/css2?family=Galada&display=swap');

    header{
        position: fixed;
        top:0;
        left:0;
        width: 100%;
        
        /* height: 50px; */
        background: var(--accent);
        z-index: 100;
    }
    .wrapper{
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    }
    .svg_container{
        width: 150px;
    }
    .wrapper h1{
        padding: 0.2em;
        margin: 0;
        font-family: 'Galada', cursive;
        font-weight: 400;
        font-size: 2.5rem;

    }
</style>