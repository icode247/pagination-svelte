<script>
  import Header from "./components/Header.svelte";
  import CardLayout from "./components/CardLayout.svelte";
  import CardItem from "./components/CardItem.svelte";
  import { paginate, PaginationNav, LightPaginationNav } from "svelte-paginate";
  import Overlay from "./components/Overlay.svelte";
  import { ApolloClient, InMemoryCache, gql, HttpLink } from "@apollo/client";
  import { setClient } from "svelte-apollo";
  import { query } from "svelte-apollo";
  import { onMount } from "svelte";

  const client = new ApolloClient({
    cache: new InMemoryCache(),
    uri: "https://d33ip4f514s3iy.cloudfront.net/cms/read/en-US",
    headers: {
      Authorization: `Bearer a65bc94d4d3ae90711af1c46491d33c2dbe2a8ff15c9c8f2`,
    },
  });
  setClient(client);

  const GET_BLOGS = gql`
    query getBlogs($limit: Int, $after: String) {
      listBlogs(limit: $limit, after: $after) {
        data {
          title
          coverImage
          content
        }
        meta {
          cursor
          hasMoreItems
        }
        error {
          message
          data
        }
      }
    }
  `;

  let items = [];
  let currentPage = 1;
  let pageSize = 3;
  let cursors = [null];
  const page = 0;

  const blogs = query(GET_BLOGS, {
    variables: {
      limit: 2,
      after: cursors[page],
    },
  });

  onMount(async () => {
    const { data } = await blogs.result();
    const pointer = data.listBlogs.meta?.cursor;

    if (pointer !== cursors[page]) {
      cursors = [...cursors, pointer];
    }
  });
  // (async () => {

  //   console.log(loading, data, meta)
  //   //   const result = await blogs.result();
  //   //   console.log(result.data.listBlogs.data)
  //   //   items = result.data.listBlogs.data;
  // })();
  // $: paginatedItems = paginate({ items, pageSize, currentPage });
</script>

<main>
  <Header />
  {#if $blogs.loading}
    <Overlay />
  {:else}
    <div class="container">
      <CardLayout>
        {#each $blogs.data.listBlogs.data as item}
          <CardItem {item} />
        {/each}
      </CardLayout>
    </div>
  {/if}

  <!-- Pagination 
    <div class="pagination">
  
      <LightPaginationNav
        totalItems={items.length}
        {pageSize}
        {currentPage}
        limit={1}
        showStepOptions={true}
        on:setPage={(e) => (currentPage = e.detail.page)}
      />
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
  </div>-->
</main>

<!-- <style>
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
</style> -->
