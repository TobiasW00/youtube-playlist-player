<svelte:options tag="youtube-item-playerx" /> 
<script>
    export let video;    
</script>

{ #if video !== undefined}
<iframe title={video.title} src={`https://www.youtube.com/embed/${video.videoId}?autoplay=1`} frameBorder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowFullScreen></iframe>
{/if}
