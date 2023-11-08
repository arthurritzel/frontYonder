<script>
    import ButtonDelet from "./ButtonDelet.svelte";
    export var url = ""
    export var dadosJS = []
    export async function get(){
        var dados = await fetch("http://localhost:8080/"+url, {
            method: "GET",
            headers: {
            'Content-Type': 'application/json'
        }
        })
        dadosJS = await dados.json();
        console.log(dadosJS, "teste")
    }
    get()
    
</script>

<div class="container flex justify-around ">
    <div class="bg-[#D4CDC5] p-10 rounded-2xl w-1/3">
        {#if url == "usuarios"}
            {#each dadosJS as dado}
                <p class="border-4 border-[#D4CDC5] border-b-black text-2xl p-1 flex justify-between">{dado.nome} <button on:click={get}><ButtonDelet id="{dado.id}" table={url}></ButtonDelet></button></p>
                
            {/each}
        {/if}
    </div>
</div>
