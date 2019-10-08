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

</style>


<div on:click={handleClick} title="Play"><img width="360px" height="270px" loading="lazy" alt={video.title} src={video.image} /><div><img src="/pictures/playvideo.png" alt="Video abspielen" /></div></div>
