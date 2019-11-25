<script>
    import { currentviedeo } from '../stores.js';
    export let video;

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

<style>
.playerpreviewbox
{
width:100%;
text-align: center;
cursor:pointer;
}
.playimage
{
    position:relative;
    top:-150px;
    z-index: 10;
    opacity: .6;
}
</style>


<div class="playerpreviewbox" on:click={handleClick} title="Play"><img  width="100%" height="100%" loading="lazy" alt={video.title} src={video.image} /><img class="playimage" src="/pictures/playvideo.png" alt="Video abspielen" /></div>
