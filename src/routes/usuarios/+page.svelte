<script>
    import Api from "$lib/Lista.svelte"
    // import dadosJS from "$lib/Lista.svelte"
    import FormUsu from "$lib/FormUsu.svelte";
    
</script>

<p class="text-center text-4xl py-9 underline decoration-sky-500">Gerenciar Usuarios</p>
<Api url="usuarios"></Api>

<FormUsu></FormUsu>

