<svelte:options tag="youtube-item"  /> 

<script>
import { onMount } from 'svelte';
import { currentviedeo } from '../stores.js';
	import VideoPlayer from './player.svelte';
	import VideoPreview from './playerpreview.svelte';
	import Autolinker from 'autolinker';
	export let video;
    let currentviewo_value;

const unsubscribe = currentviedeo.subscribe(value => {
		currentviewo_value = value;
    });
	
	let descriptioncss = "cursor:pointer";

function handleDescriptionClick()
{
	descriptioncss = "overflow-y:scroll;";
}



</script>

{ #if video !== undefined}
<div class="youtube-box-div">
<div class="youtubeheader">
<h3>{video.title}</h3>
</div>
<div>
{ #if video.id === currentviewo_value}
<VideoPlayer video={video} />
{:else}
<VideoPreview video={video} />
{/if}
<div class="description" style={descriptioncss} on:click={handleDescriptionClick}>
{@html Autolinker.link(video.description)}
</div>

      
</div>
</div>
{/if}

