<script>
//	export let name;
  import { onMount } from "svelte";
  import Youtube from './components/Youtube.svelte';
let videocount =0;
let videos = [];


		async function getYouTubeVideos(youtubetoken) {
			let url = 'https://www.googleapis.com/youtube/v3/playlistItems?part=snippet&maxResults=8&playlistId='
        url = (youtubetoken.length===0) ? url : url + "&pageToken="  + youtubetoken;
		const res = await fetch(url);
		const json = await res.json();
		if (res.ok) {
				videocount = json.pageInfo.totalResults;
				let temp = [];
				json.items.forEach((video,i)=>{
							try{
							let webpimageurl = video.snippet.thumbnails.high.url;
							temp.push({id:video.id,title:video.snippet.title,description:video.snippet.description,image:webpimageurl,videoId:video.snippet.resourceId.videoId});			
			
							if(i===3 || i===7)
							{
							videos = [...videos,temp];	
							temp = [];
							}
							}catch(err)
							{
							// console.log(video);
							//PLy5WSqnI1QT2pIf0CFW5NEUP0UbwFxBRD&key=AIzaSyBn8oOlCCABDHovkRqgATMAzhhpewnZqB4
							}
							});
		  if(json.pageInfo.totalResults > videos.length){
           loadMoreTrigger(json.nextPageToken);
		   }
		



		} else {
			throw new Error(text);
		}
	}

	  onMount(async function() {
			getYouTubeVideos("");
	 });



function loadMoreTrigger(newyoutubekotekn)
      {
   const sentinel =  document.getElementById("loadmore");
   const config = {
     rootMargin: '300px 0px',
     threshold: 0.01
   };
   
   let observer = new IntersectionObserver(onIntersection, config);
   observer.observe(sentinel);
   
   function onIntersection(entry) {
     if (entry[0].intersectionRatio > 0) {
       observer.unobserve(entry[0].target);
      getYouTubeVideos(newyoutubekotekn);
     }
   } 
   
   }
 
</script>

<style>

</style>

<div>
<div>{videos.length}</div>

<div>{videocount}</div>
<div id="youtube-box-div">
<table>
	<tr>
			{#each [0,1,2,3] as row}
			<td>
				{#each videos as video}
					<Youtube video={video[row]} />
				{/each}
			</td>
			{/each}
	</tr>
</table>
</div>
	<div id="loadmore" />
</div>
