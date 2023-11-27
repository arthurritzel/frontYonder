<script>
    import Api from "$lib/ListaResp.svelte"
   
    
</script>

<p class="text-center text-4xl py-9 underline decoration-sky-500">Gerenciar Respostas</p>
<div class="container flex justify-around ">
<Api url="respostas"></Api>
</div>

