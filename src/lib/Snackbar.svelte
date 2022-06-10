
<script>

import { Snackbar, Button, MaterialApp } from 'svelte-materialify';

export let show,type,message,buttonText,timeout = 3000;


let snackbar = true;


</script>
    <MaterialApp>
        <Snackbar class="justify-space-between {type}-color" bottom right bind:active={show}  timeout={timeout}>
            {message}
            <Button
              text
              on:click={() => {
                show = false;
              }}>
              {buttonText}
            </Button>
          </Snackbar>
</MaterialApp>