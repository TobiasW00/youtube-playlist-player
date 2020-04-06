<svelte:options tag="youtube-player" />
<script>

import { afterUpdate } from "svelte";
import Youtube from './components/Youtube.svelte';
import { fly } from 'svelte/transition';
export let playlistid ="PL94TjndaH-lgl7C3irKhANeonE8tmu3gi";
export let key="AIzaSyCsD7IaJ0v7IoOlfBGKyDJDrYYAl8KFQCI";
let videocount =0;
let videos = [];
let windowwidth = window.innerWidth;
$: colcount =  Math.round( windowwidth/ 330)-1;
$: colwidth =25;
$: switch(colcount)
{
	case 0:
	colwidth = 100;
	case 1:
	colwidth = 100;
	break;
	case 2:
	colwidth = 48;
	break;
	case 3:
	colwidth = 32;
	break;
    case 4:
	colwidth = 23;
	break;
    default:
		colwidth = 15;

}

async function getYouTubeVideos(youtubetoken) {
		let url = 'https://www.googleapis.com/youtube/v3/playlistItems?part=snippet&maxResults=' + 10  +"&playlistId="+ playlistid + "&key=" + key;
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
							if(video.snippet.thumbnails)
							{	
							let webpimageurl = video.snippet.thumbnails.high.url;
							
							const v = {id:video.id,title:video.snippet.title,description:video.snippet.description,image:webpimageurl,videoId:video.snippet.resourceId.videoId};
							//const v = {id:video.id.videoId,title:video.snippet.title,description:video.snippet.description,image:webpimageurl,videoId:video.id.videoId};
							videos = [...videos,v];	
							}
							});

		  if(json.pageInfo.totalResults > videos.length){
           loadMoreTrigger(json.nextPageToken);
		   }
		} else {
			console.log(res);
		}
		}

			afterUpdate(() => {
				console.log("afterUpdate");
				getYouTubeVideos("");
				console.log(key);
			});
/*
onMount(async function() {
			getYouTubeVideos("");
	 });
*/


function loadMoreTrigger(newyoutubekotekn)
      {
return;
   const sentinel =  document.getElementById("loadmore");
	console.log(sentinel);
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


<svelte:window bind:innerWidth={windowwidth}/>
<div>
	<div id="youtube-box-div">	
	{#each videos as videovalue}			
				<div class="youtubebox" transition:fly="{{ y: 200, duration: 2000 }}" style="width:{colwidth}%">
      	<Youtube  video={videovalue} />	
				</div>	
	{/each}
	</div>
	<div id="loadmore" />
</div>

<link rel="stylesheet" href="design.css" />