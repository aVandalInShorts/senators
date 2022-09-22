<template>
	<div class="overflow-hidden bg-white shadow sm:rounded-lg main">
		<div class="border-b-2 border-gray-200 p-4">
			<h1 class="text-lg font-medium text-center text-gray-900 uppercase">Senators</h1>
			<p class="text-center text-gray-500">Do you recognize them all?</p>
		</div>
		
		<div class="p-4 text-center">
			<nuxt-link to="/game" class="inline-block p-2 rounded-md border border-gray-300 hover:bg-gray-500 hover:text-gray-50 hover:border-greay-500 transition ease-in-out duration-200">Play the game</nuxt-link>
		</div>
	</div>
</template>

<script>
export default {
	
}
</script>

<style>
	.main {
		width: min(92%, 600px);
	}
</style>