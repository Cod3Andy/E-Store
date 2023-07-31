<script lang="ts">
	import ProductCard from "$lib/productCard.svelte";
	import { get } from "svelte/store";
	import { cartItems } from "../cart";

	const products: Product[] = [
		{
			id: "API_ID_FOR_A_PRODUCT_HERE_1",
			name: "Coffee",
			price: 5,
		},
		{
			id: "API_ID_FOR_A_PRODUCT_HERE_2",
			name: "Tea",
			price: 5,
		},
		{
			id: "API_ID_FOR_A_PRODUCT_HERE_3",
			name: "Milk",
			price: 5,
		},
	];
	async function checkout(){
		await fetch("api/stripeCheckout",{
			method: "POST",
			headers: {
				"Content-Type": "application/json"
			},
			body: JSON.stringify(
				{ items: get(cartItems) }
			)
		}).then((data) => {
			return data.json()
		}).then((data) => {
			window.location.replace(data.url);
		})
	}
</script>

<div class="container h-full mx-auto flex justify-center items-center">
	<div class="grid grid-col-3 gap-4">
		<div class="col-span-3">
			<h1 class="text-4xl">SveltKit 1.0 Store</h1>
		</div>
		{#each products as product}
		<ProductCard product={product}/>
		{/each}
		<div class="col-span-3">
			<button on:click={() => checkout()} class="btn variant-filled-primary">Checkout with Stripe API</button>
		</div>
	</div>
</div>
