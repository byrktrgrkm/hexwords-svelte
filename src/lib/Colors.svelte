<script>
    export let data;
    export let snackbar;


    import Color from './Color.svelte'
    import Func from '../func/helper'

    data = data.filter(element => element[1].length == 4  || element[1].length == 7 || element[1].length == 9);
    let copy = [...data];

    let searchField,colorTextField,colorField;

    const changed = () => {
        const s = searchField.toLowerCase();
        copy = data.filter(element => element[0].toLowerCase().includes(s) || element[1].toLowerCase().includes(s));
    }
    
    const changedColorText = () => {
        const s = colorTextField.toLowerCase();
        var test1 = /^#?([a-f\d]{1})([a-f\d]{1})([a-f\d]{1})$/i.test(s);
        var test2 = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.test(s);
        if(test1 || test2){
            colorField = s;
            changeColorInput();
            //Func.
        }else{
            console.info("#rrggbb formatında olmalıdır.");
        }
        
    }

    const changeColorInput = () => {
        colorTextField = colorField;
        let rgb = Func.hexToRgb(colorTextField);
        
   
        copy = data
        .filter(element =>  Func.deltaE( Func.hexToRgb(element[1]), rgb)  < 40)
        .map(element => { element[2] = Func.deltaE( Func.hexToRgb(element[1]), rgb); return element })
        .sort((a,b) => a[2] - b[2]);

    }
    
    </script>


<div class="search-field">
    <input type="text" bind:value={searchField} on:input={changed} placeholder="Arama yap" />
    <div class="proximity ">
        <input type="text" bind:value={colorTextField} on:input={changedColorText} placeholder="Benzer renkleri bul" />
        <input bind:value={colorField} type="color" on:focusout={changeColorInput}/>
    </div>
</div>

<ul>
{#each copy as c, i}
    <Color color={c} bind:snackbar={snackbar}/>
{/each}

</ul>


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

    input[type="text"]{
        width: min(30rem,50vw);
        display: block;
        box-shadow: 0 0 0.3rem #0000004d;
        outline: 0;
        border: 0;
        border-radius: 10rem;
        padding: 0.3rem 0.7rem;
    }

    .search-field{
        position: sticky;
        top: 0;
        background-color: var(--selected-color);
        padding-block: 1rem;
        display: grid;
        justify-content: center;
        align-items: center;
        gap: 1rem;
        grid-template-columns: max-content max-content max-content;
        z-index: 10;
    }
    
    .proximity{
        grid-column: 1/-1;
    display: grid;
    grid-template-columns: 1fr min-content;
    }

    input[type="color"]{
        outline: 0;
    border: 0;
    }
</style>