<script>
    export let video;
    
</script>

<style>

</style>


<iframe title={video.title} src={`https://www.youtube.com/embed/${video.videoId}?autoplay=1`} frameBorder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowFullScreen></iframe>
