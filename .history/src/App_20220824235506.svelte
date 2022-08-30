<script>
  import Header from "./components/Header.svelte";
  import CardLayout from "./components/CardLayout.svelte";
  import CardItem from "./components/CardItem.svelte";
  import { paginate, PaginationNav, LightPaginationNav } from "svelte-paginate";
  import Overlay from "./components/Overlay.svelte";
  import { ApolloClient, InMemoryCache, HttpLink } from "@apollo/client";

    const client= new ApolloClient({
      uri: "https://d33ip4f514s3iy.cloudfront.net/cms/read/en-US",
      cache: InMemoryCache(),
      headers: {
        Authorization: `Bearer ${authToken}`,
      },
    });
	setClient(client);

  let items = [];
  let currentPage = 1;
  let pageSize = 6;
  let loading = true;

  (async () => {
    await fetch("https://jsonplaceholder.typicode.com/photos?_limit=100")
      .then((res) => res.json())
      .then((data) => {
        items = data;
        loading = false;
      });
  })();

  $: paginatedItems = paginate({ items, pageSize, currentPage });
</script>

<main>
  <Header />
  {#if loading}
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
