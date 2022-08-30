<script>
	import Header from "./components/Header.svelte";
	import CardLayout from "./components/CardLayout.svelte";
	import CardItem from "./components/CardItem.svelte";
	import { paginate, PaginationNav } from 'svelte-paginate';
	import Overlay from "./components/Overlay.svelte";

  let items = [];
  let currentPage = 1;
  let pageSize = 6;
	let loading = true;

	(async () => {
		await fetch("https://jsonplaceholder.typicode.com/photos?_limit=100").then(res => res.json()).then(
			data => {
				items = data;
				loading = false;
			}
		);
	})();

  $: paginatedItems = paginate({ items, pageSize, currentPage })

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
		
		<div class="pagination">
			<PaginationNav
				totalItems="{items.length}"
				pageSize="{pageSize}"
				currentPage="{currentPage}"
				limit="{1}"
				showStepOptions="{true}"
				on:setPage="{(e) => currentPage = e.detail.page}"
			>
				<span slot="prev">
					Prev
				</span>
				<span slot="next">
					Next
				</span>
			</PaginationNav>
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
	.pagination :global(.option){
		color: rgb(51, 51, 51);
		padding: 10px 7px;
		cursor: pointer;
	}

	@media(min-width: 992px){
		.pagination :global(.option){
			padding: 10px 15px !important;
		}
	}

	.pagination :global(.option.active){
		color: #fff;
		background: rgb(38, 93, 151);
	}

.pagination :global(.option.disabled){
	color: #999;
	cursor: not-allowed;
}

</style>