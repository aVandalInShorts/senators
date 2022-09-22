<template>
	<button 
		type="button" 
			class="p-3 bg-white flex align-center transition ease-in-out duration-200" 
			:class="{ 'bg-green-300' : state === 'highlight', 'bg-red-600' : state === 'error', 'bg-green-600' : state === 'correct', 'opacity-50' : disabled, 'hover:bg-gray-200' : state == 'normal' }"
			:disabled="state !== 'normal'"
			@click="$emit('clicked', num)">
		<span class="inline-block bg-sky-600 w-[25px] h-[25px] text-white rounded-full">{{ num }}</span>
		<span class="inline-block ml-2">{{ name }}</span>
	</button>
</template>
<script setup>
	defineProps({
		num: Number,
		name: String,
		state: String,
		disabled: Boolean
	});
</script>
<style lang="">
	
</style>