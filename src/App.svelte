<script>
//	export let name;
import { onMount } from "svelte";
import Youtube from './components/Youtube.svelte';
let videocount =0;
let videos = [];
let windowwidth = window.innerWidth;
$: colcount =  Math.round( windowwidth/ 330)-1;
$: videosordered = ordered(videos,colcount);

const ordered = (arr, columns) => {
        const cols = columns;
        const out = [];
        let col = 0;
        while(col < cols) {
            for(let i = 0; i < arr.length; i += cols) {
                let _val = arr[i + col];
               if (_val !== undefined)
                    out.push(_val);
            }
            col++;
        }
		return out;
}

async function getYouTubeVideos(youtubetoken) {
		let url = 'https://www.googleapis.com/youtube/v3/playlistItems?part=snippet&maxResults=' + colcount  +'&playlistId=PL94TjndaH-li6mJbaucN3lPaSuq-X9Jot&key=AIzaSyBn8oOlCCABDHovkRqgATMAzhhpewnZqB4'
 	    url = (youtubetoken.length===0) ? url : url + "&pageToken="  + youtubetoken;
		const res = await fetch(url);
		const json = await res.json();
		if (res.ok) {
				videocount = json.pageInfo.totalResults;
				json.items.filter((video,i)=>{
							try{
								return true;
							}catch(err)
							{
							 return false
							}
							}).forEach((video) =>
							{
							let webpimageurl = video.snippet.thumbnails.high.url;
							const v = {id:video.id,title:video.snippet.title,description:video.snippet.description,image:webpimageurl,videoId:video.snippet.resourceId.videoId};
							videos = [...videos,v];	
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
#youtube-box-div > div
{
	vertical-align: top;
}
</style>
<svelte:window bind:innerWidth={windowwidth}/>
<div>
<div>{videos.length}</div>
<div>{windowwidth}</div>
<div>colcount {colcount}</div>

<div id="youtube-box-div" style="column-count: {colcount};">	
			{#each videosordered as video,i}
				<div>
				{i}
				<Youtube index={video.id} video={video} />
				</div>
			{/each}
</div>
<div id="loadmore" />
</div>
