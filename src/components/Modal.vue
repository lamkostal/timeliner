<template>
  <div class="modal_container" v-if="modalOpen" @click="closeModal">
      <div class="modal_content">
          <p>Welcome to simple timeliner</p>
          <span>a tool to create timelines</span>
          <ul>
          
            <li>Sorted entries by date</li>
            <li>Choose date format and language</li>
            <li>Customize colors</li>
            <li>Export your timeline as png, jpeg or svg</li>
            <li>Save your timeline for later, it will be there when you visit again*</li>
          </ul>
            <p>Enjoy!</p>
            <span class="asterisc">*saving uses browsers local storage, erasing browsing data will affect your saves.</span>

      </div>
  </div>
</template>

<script>
export default {
data(){
    return{
        modalOpen:false
    }
},
methods:{
    closeModal(){
        this.modalOpen=false
        this.$emit('modalClose')
    }
},
mounted(){
    this.modalOpen=true
}

}
</script>


<style>
.asterisc{
    font-size: 0.8rem;
    position: relative;
    padding:  0 1em;
    bottom:0;
    left: 0;
}
    .modal_container{
        position: fixed;
        width: 100%;
        height: 100vh;
        background: rgba(0, 0, 0, 0.24);
        color:var(--text);
        display: flex;
        justify-content: center;
        align-items: center;
        z-index: 1000;
       
    }
    .modal_content{
        max-width: 80vw;
        background: var(--accent);
        /* height: 50vh; */
        padding: 1em ;
        display: flex;
        flex-direction: column;
        justify-content: center;
        gap:10px;
        border-radius: 15px;
        box-shadow: 1px 1px 15px rgba(0, 0, 0, 0.423) ;
        font-size: 1.2rem;
    }
    .modal_content p{
         font-family: 'Galada';
         font-size: 3.5vh;
         padding: 0;
        color:var(--btn-active);

     }
     .modal_content ul{
         list-style: inside;
         max-width: 60ch;
         
     }
      .modal_content li{
         /* max-width:90vw; */
         padding: 0.4rem  ;
         margin-left: 1em;
         width:auto;
     }
      .modal_content li::marker{
          content:'✔  ';
         color:var(--btn-active);
         font-size: 1.5em;
      }


</style>