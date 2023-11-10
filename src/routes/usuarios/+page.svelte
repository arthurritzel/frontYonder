<script>
    import Api from "$lib/ListaUsu.svelte"
   
    
</script>

<p class="text-center text-4xl py-9 underline decoration-sky-500">Gerenciar Usuarios</p>
<div class="container flex justify-around ">
<Api url="usuarios"></Api>
</div>

