<script>
    import Api from "$lib/ListaEmp.svelte"
</script>

<p class="text-center text-4xl py-9 underline decoration-sky-500">Gerenciar Empresas</p>

<div class="container flex justify-around ">
    <Api url="empresas"></Api>
</div>