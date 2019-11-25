<script>
    export let video;
    
</script>

<style>
iframe
{
width:100%;
text-align: center;
cursor:pointer;
height:280px;
padding-bottom: 60px;
}
</style>

<iframe title={video.title} src={`https://www.youtube.com/embed/${video.videoId}?autoplay=1`} frameBorder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowFullScreen></iframe>
