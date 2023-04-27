<script>
	import { beforeNavigate } from '$app/navigation'
	import { links } from './Header/Nav/Nav.svelte'
	import { quintOut } from 'svelte/easing'
	import { fly } from 'svelte/transition'

	const duration = 1000
	let fullWidth = true

	$: distance = fullWidth ? globalThis.window?.innerWidth ?? 250 : 250

	$: outOpts = { duration, easing: quintOut, x: distance }
	$: inOpts = { duration, easing: quintOut, x: distance, delay: duration / 4 }

	beforeNavigate((e) => {
		// Add home ('/') to the links list.
		const allLinks = ['/', ...links.map(([path]) => path)]

		const currentIndex = allLinks.findIndex((path) => path == String(e.from?.route.id))
		const nextIndex = allLinks.findIndex((path) => path === String(e.to?.route.id))

		const direction = currentIndex < nextIndex ? 1 : -1

		outOpts.x = Math.abs(outOpts.x) * direction
		inOpts.x = Math.abs(inOpts.x) * -direction
	})
</script>

<p style:margin-left="5rem">
	Full Width Transitions <input type="checkbox" bind:checked={fullWidth} />
</p>

{#key outOpts}
	<section out:fly={outOpts} in:fly={inOpts}>
		<div class="br-md" />
		<slot />
	</section>
{/key}

<style lang="scss">
	section {
		position: absolute;
		inset: 0;

		width: 100%;
		height: 100%;
		margin: auto;
	}
</style>
