<script lang="ts">
	import './layout.css';
	import { Notification, Tooltip } from 'theui-svelte';
	import favicon from '#lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<!--
	Notification and Tooltip each drive every instance on the page, so they are
	rendered once here rather than per route. A second copy of either duplicates
	its listeners. notify() posts into this one from anywhere; tooltips are opt-in
	per element with data-tooltip="...".
-->
<Notification position="top-end" />
<Tooltip />

{@render children()}
