<script>
  import { fade } from 'svelte/transition';
</script>

<div class="overlay" transition:fade="{{delay: 0, duration: 300}}">
  <p>Loading...</p>
</div>

<style>
  .overlay{
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }
  .overlay p{
    font-size: 20px;
  }
  @media(min-width: 992px){
    .overlay p{
      font-size: 40px;
    }
  }
</style>