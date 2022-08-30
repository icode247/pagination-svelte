<script>
  export let item;
</script>

<div class="card">
  <img src={item.url} alt="" class="img-fluid">
  <h3 class="__title">{item.title}</h3>
  <p class="__text">
    <!-- {item.body.substring(0, 80)} -->
    quia et suscipit suscipit recusandae consequuntur expedita et cum reprehenderit molestiae ut ut quas totam nostrum rerum.
  </p>
  <a href={`https://jsonplaceholder.typicode.com/posts/${item.id}`}>Read more</a> 
</div>
  
<style>
  .card {
    display: flex;
    flex-direction: column;
    gap: 20px;
    border: 1px solid #b2bac2;
    background-color: #132f4c;
    color: #fff;
    border-radius: 15px;
    padding: 1rem;
    position: relative;
  }
  img{
    height: 160px;
    width: 100%;
    object-fit: cover;
  }
  .__title{
    font-size: 1rem;
    font-weight: medium;
    width: 200px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .__text{
    font-size: 0.9rem;
    font-weight: normal;
  }
  a{
    background: #8EA0F3;
    color: #fff;
    text-align: center;
    border-radius: 5px;
    padding: 5px;
  }
</style>