<script>
import { bind } from "svelte/internal";



    export let color,snackbar;

 

    const copyValue = function(){
            let color = this.dataset.color;
            copyTextToClipboard(color,function(){
                snackbar.do( `${color} rengi başarıyla kopyaladı.`,'success');
            },function(){
                snackbar.do( `${color} kopyalanırken bir hata ile karşılaşıldı..`,'error');
            })
        
    }

    function copyTextToClipboard(text,success,error) {
        var textArea = document.createElement("textarea");
        textArea.style.position = 'fixed';
        textArea.style.top = 0;
        textArea.style.left = 0;

        textArea.style.width = '2em';
        textArea.style.height = '2em';


        textArea.style.padding = 0;

        // Clean up any borders.
        textArea.style.border = 'none';
        textArea.style.outline = 'none';
        textArea.style.boxShadow = 'none';
        textArea.style.background = 'transparent';


        textArea.value = text;

        document.body.appendChild(textArea);
        textArea.focus();
        textArea.select();

        try {
            var successful = document.execCommand('copy');
            successful ? success() : error();

        } catch (err) {
            error();
        }
        document.body.removeChild(textArea);
    }
    </script>


    <li style="background-color:{color[1]}">
        <button class="left" on:click={() => document.body.style.backgroundColor = color[1]}>{color[1]} <br><small> ({color[0]})</small></button>
        <button class="right" on:click={copyValue} data-color={color[1]}><svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 0 24 24" width="24px" fill="currentColor"><path d="M0 0h24v24H0V0z" fill="none"></path><path d="M16 1H4c-1.1 0-2 .9-2 2v14h2V3h12V1zm3 4H8c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h11c1.1 0 2-.9 2-2V7c0-1.1-.9-2-2-2zm0 16H8V7h11v14z"></path></svg></button>
    </li>


    <style>

    
    
    ul{
        margin: auto;
        padding: 0;
        list-style: none;
        display: grid;
        justify-content: center;
        gap: 0.5rem;
        grid-template-columns: repeat(auto-fit,minmax(15rem,1fr));
        max-width: 90vw;
    }
    
    li{
        --_padding: .5rem;
        text-align: center;
        font-weight: 700;
        background-color: var(--color, transparent);
        padding: var(--_padding);
        border-radius: 0.5rem;
        position: relative;
    }

    button.left{
        width: 100%;
        height: 100%;
        background: transparent;
        border: 0;
        font-family: monospace;
        cursor: pointer;
    }
    button.right{
        position: absolute;
        top: 50%;
        right: var(--_padding);
        width: fit-content;
        padding: 0;
        height: auto;
        transform: translateY(-50%);
        background: transparent;
        border: 0;
        font-family: monospace;
        cursor: pointer;
    }
    button small{
        font-size: .6rem;
    }




    </style>