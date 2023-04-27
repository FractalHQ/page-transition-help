<script lang="ts" context="module">
	export const links = [
		['/about', 'About'],
		['/contact', 'Contact'],
		['/cats', 'Cats']
	] as const
</script>

<script lang="ts">
	import { setContext } from 'svelte'
	import { mobile } from 'fractils'

	import Mobile from './Mobile/Mobile.svelte'
	import Desktop from './Desktop.svelte'

	setContext('links', links)
</script>

<template lang="pug">

	+if('$mobile')

		Mobile

		+else
			Desktop

</template>
