<div class="container" style="margin-top:10rem;">
	<Tags bind:tags />
</div>

<script>
	import Tags from "./components/Tags.svelte";

	export let tags = [];
</script>

<style global lang="scss">
	@import "./scss/main.scss";
</style>