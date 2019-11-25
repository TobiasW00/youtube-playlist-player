<script>
    import { currentviedeo } from '../stores.js';
	import VideoPlayer from './player.svelte';
	import VideoPreview from './playerpreview.svelte';
	import Autolinker from 'autolinker';
	export let csswidth;
	export let video;
    export let index;
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

<style>
	.youtube-box-div {
		display: inline-block;
		border:3px solid black;
		border-radius:5%;
		margin:1%;
		width:100%;
		overflow:hidden;
		text-align: center;
	}
	.description
	{
		text-align: left;
		/*overflow-y: scroll; */
		overflow: hidden;
		margin-top: -60px;
		font-size: .9em;
		max-height: 300px;
		scrollbar-width: thin;	
	}
	.youtubeheader
	{
		background:red;
		color:white;
	}
	h3
	{
			margin:0;
	}

</style>

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
{Autolinker.link(video.description)}
</div>
      
</div>
</div>