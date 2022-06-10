<script>

import Colors from './lib/Colors.svelte'

import Snackbar from './lib/Snackbar.svelte'

/**
 * 
 * 
 * import { onMount } from 'svelte';
 * 
 * 	let colorsData = [];
 * 
 * onMount(async () => {
		const res = await fetch(`https://gist.githubusercontent.com/f/9c59c515fca028b549a6014aa43c14b0/raw/8253074695e0777b97b6be5e96b6c51f645337ae/all-hex-words.json`);
		colorsData = await res.json();
	});
 * 
 * 
 */
async function getColors() {
		const res = await fetch(`https://gist.githubusercontent.com/f/9c59c515fca028b549a6014aa43c14b0/raw/8253074695e0777b97b6be5e96b6c51f645337ae/all-hex-words.json`);
		const json = await res.json();

		if (res.ok) {
			return json;
		} else {
			throw new Error(json);
		}
	}

let colorsData = getColors();


let snackbarMessage = {
  message: '',
  buttonText:'Kapat',
  show:false,
  type:'success',
  timeout:3000,
  do:(message,type) => {
    snackbarMessage.show = false;
    snackbarMessage.message = message;
    snackbarMessage.type = type;
    snackbarMessage.show = true;
  }
};


</script>

<h3>HEXWORDS - TR</h3>

<p>proje kaynağı <a target="_blank" href='https://github.com/byrktrgrkm/hexwords-svelte'>github</a></p>
{#await colorsData}
	<p>Veriler yükleniyor ..</p>
{:then data}
	<Colors data={data}  bind:snackbar={snackbarMessage} />
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}


<Snackbar {...snackbarMessage} />

<style>
  :root {
    --color: #0FF1CE;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
      Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }
 body{

  margin: 0;
    padding: 0;
    font-family: monospace;
 }

 h3,p{
   text-align: center;
 }
</style>
