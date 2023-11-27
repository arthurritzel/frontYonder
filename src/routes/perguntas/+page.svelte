<script>
    import Api from "$lib/ListaPergunta.svelte"
   
    
</script>

<p class="text-center text-4xl py-9 underline decoration-sky-500">Gerenciar Perguntas</p>
<div class="container flex justify-around ">
<Api url="perguntas"></Api>
</div>

