<script>
  import Header from "./components/Header.svelte";
  import CardLayout from "./components/CardLayout.svelte";
  import CardItem from "./components/CardItem.svelte";
  import { paginate, PaginationNav, LightPaginationNav } from "svelte-paginate";
  import Overlay from "./components/Overlay.svelte";
  import { ApolloClient, InMemoryCache, gql, HttpLink } from "@apollo/client";
  import { setClient } from "svelte-apollo";
  import { query } from "svelte-apollo";
  import { createEventDispatcher } from 'svelte'

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

  let cursors = [];
  let page = 0;
  let blogs;

 $: { const blogsData = query(GET_BLOGS, {
    variables: {
      limit: 2,
      after: cursors[page],
    },
  });
  
}

  $: (async () => {
    const { data } = await blogs.result();
    const pointer = data.listBlogs.meta?.cursor;

    if (pointer !== cursors[page]) {
      cursors = [...cursors, pointer];
    }
    console.log(cursors, page);
  })();
</script>

<main>
  <Header />
  <!-- svelte-ignore missing-declaration -->
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

  <!-- svelte-ignore missing-declaration -->
  {#if !$blogs.loading}
    <p>{page}</p>
    <div class="next-prev">
      <button
        disabled={page === 0 && true}
        style={{ backgroundColor: `${page === 0 ? "gray" : "#6796ec"}` }}
        on:click={() => page -= 1}>Prev</button
      >
      <button
        disabled={!$blogs?.data.listBlogs.meta?.hasMoreItems && true}
        style={{
          backgroundColor: `${
            $blogs?.data?.listBlogs.meta?.hasMoreItems ? "#6796ec" : "gray"
          }`,
        }}
        on:click={() => (page += 1)}>Next</button
      >
    </div>
  {/if}
</main>
