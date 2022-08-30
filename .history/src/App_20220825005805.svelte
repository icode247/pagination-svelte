<script>
  import Header from "./components/Header.svelte";
  import CardLayout from "./components/CardLayout.svelte";
  import CardItem from "./components/CardItem.svelte";
  import { paginate, PaginationNav, LightPaginationNav } from "svelte-paginate";
  import Overlay from "./components/Overlay.svelte";
  import { ApolloClient, InMemoryCache, gql, HttpLink  } from "@apollo/client";
  import { setClient } from "svelte-apollo";
  import { query } from "svelte-apollo";
  const client = new ApolloClient({
    cache: new InMemoryCache(),
    uri: "https://d33ip4f514s3iy.cloudfront.net/cms/read/en-US",
     headers: {
      Authorization: `Bearer a65bc94d4d3ae90711af1c46491d33c2dbe2a8ff15c9c8f2`,
    },
  });
  setClient(client);

  const GET_BLOGS = gql`
    query getBlogs {
      listBlogs {
        data {
          title
        }
      }
    }
  `;

  const blogs = query(GET_BLOGS);
 (
	
 )
  let items = [];
  let currentPage = 1;
  let pageSize = 6;
  let loading = true;

//   (async () => {
//     await fetch("https://d33ip4f514s3iy.cloudfront.net/cms/read/en-US", {
// 		method: "POST",
// 		headers:{
// 			Authorization: `Bearer a65bc94d4d3ae90711af1c46491d33c2dbe2a8ff15c9c8f2`,
// 		}
// 	})
//       .then((res) => res.json())
//       .then((data) => {
// 		console.log(data)
//         items = data;
//         loading = false;
//       });
//   })();

  $: paginatedItems = paginate({ items, pageSize, currentPage });
</script>

<main>
	
	  {console.log($blogs)}
	
  <Header />
  {#if $blogs.loading}
    <Overlay />
  {/if}
  <div class="container">
    <CardLayout>
      {#await paginatedItems}
        <Overlay />
      {:then items}
        {#each items as item}
          <CardItem {item} />
        {/each}
      {/await}
    </CardLayout>

    <!-- Pagination -->
    <div class="pagination">
      <!-- Shows angles for prev/next -->
      <LightPaginationNav
        totalItems={items.length}
        {pageSize}
        {currentPage}
        limit={1}
        showStepOptions={true}
        on:setPage={(e) => (currentPage = e.detail.page)}
      />

      <!-- Shows prev/next text -->
      <div class="pagination_nav">
        <PaginationNav
          totalItems={items.length}
          {pageSize}
          {currentPage}
          limit={1}
          showStepOptions={true}
          on:setPage={(e) => (currentPage = e.detail.page)}
        >
          <span slot="prev"> Prev </span>
          <span slot="ellipsis">|</span>
          <span slot="next"> Next </span>
        </PaginationNav>
      </div>
    </div>
  </div>
</main>

<style>
  .pagination :global(.pagination-nav) {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 20px;
  }
  .pagination_nav :global(.option.number) {
    display: none;
  }
  .pagination :global(.option) {
    color: rgb(51, 51, 51);
    padding: 10px 7px;
    cursor: pointer;
  }

  @media (min-width: 992px) {
    .pagination :global(.option) {
      padding: 10px 15px !important;
    }
  }

  .pagination :global(.option.active) {
    color: #fff;
    background: rgb(38, 93, 151);
  }

  .pagination :global(.option.disabled) {
    color: #999;
    cursor: not-allowed;
  }
</style>
