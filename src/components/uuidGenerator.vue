<script setup lang="ts">
import { ref } from 'vue'
import { v1, v4 } from 'uuid'

const generators = [
	{ name: 'v1', fn: v1 },
	{ name: 'v4', fn: v4 },
]

const selectedGenerator = ref(generators[0].name)
const generatedUUID = ref('')
const copyBtnText = ref('Copy')

const generateUUID = () => {
	const generator = generators.find((g) => g.name === selectedGenerator.value)
	if (generator) {
		generatedUUID.value = generator.fn()
		copyBtnText.value = 'Copy'
	}
}
const copyToClipboard = () => {
	navigator.clipboard.writeText(generatedUUID.value)
	copyBtnText.value = 'Copied!'
}

</script>

<template>
<div class="flex flex-col justify-center">
	<h1 class="text-5xl font-bold">Online UUID Generator</h1>

	<div class="flex items-center ">
		<div class="form-control w-1/2 mx-1">
			<label class="label">
				<span class="label-text">Select UUID version</span>
			</label>
			<select v-model="selectedGenerator" class="select select-bordered">
				<option v-for="generator in generators" :key="generator.name" :value="generator.name">{{ generator.name }}</option>
			</select>
		</div>

		<button class="btn mt-9 mx-1" @click="generateUUID">Generate</button>
	</div>
	<div 
		v-show="generatedUUID"
		class="rounded-box bg-amber-200 mt-5 p-5 flex flex-row"
	>
		<div class="grow">
			<p>{{ generatedUUID }}</p>
		</div>
		<div >
			<button class="btn btn-sm" @click="copyToClipboard">{{ copyBtnText }}</button>
		</div>
	</div>
</div>
</template>

