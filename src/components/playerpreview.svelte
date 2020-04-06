<svelte:options tag="youtube-item-playerpreview" /> 
<script>
    import { currentviedeo } from '../stores.js';
    export let video;
    let imgheight;
    let currentviewo_value;
	const unsubscribe = currentviedeo.subscribe(value => {
		currentviewo_value = value;
    });

    function handleClick()
    {
    if(currentviewo_value !== video.id)
        {
            currentviedeo.update(n => video.id);
        }
    }

</script>


{ #if video !== undefined}
<div class="playerpreviewbox" bind:clientHeight={imgheight} on:click={handleClick} title="Play"><img width="100%" height="100%" loading="lazy" alt={video.title} src={video.image} /><img  style="top:-{imgheight/2}px;"  class="playimage" src="playvideo.png" alt="Video abspielen" /></div>
{/if}