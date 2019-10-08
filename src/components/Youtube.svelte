<script>
    import { currentviedeo } from '../stores.js';
	import VideoPlayer from './player.svelte';
	import VideoPreview from './playerpreview.svelte';
	
	export let video;
    export let index;
    	let currentviewo_value;
	const unsubscribe = currentviedeo.subscribe(value => {
		currentviewo_value = value;
    });
    



</script>

<style>
	.youtube-box-div {
		display: inline-block;
		border:3px solid black;
		border-radius:5%;
	}
</style>

<div class="youtube-box-div">
<h3>{video.title}</h3>
<div>

 { #if video.id === currentviewo_value}
<VideoPlayer video={video} />
{:else}
<VideoPreview video={video} />
{/if}
<div>
{video.description}
</div>
      
</div>
</div>