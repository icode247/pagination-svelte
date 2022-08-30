<header>
  <ul class="header_content">
    <li><a href="/">Home</a></li>
    <li><a href="/">About</a></li>
    <li><a href="/" class="active">Blog</a></li>
    <li><a href="/">Contact</a></li>
  </ul>
</header>

<style>
  header{
    background: rgb(38, 93, 151);
    color: #fff;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 1rem;
    margin-bottom: 30px;
  }
  .header_content{
    display: flex;
    gap: 20px;
    justify-content: center;
    align-items: center;
  }
  @media(min-width: 768px){
    .header_content{
      gap: 50px;
    }
  }
  a.active{
    color: #fff
  }
</style>