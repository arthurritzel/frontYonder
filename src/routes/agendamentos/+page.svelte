<script>
    import Api from "$lib/ListaAgend.svelte"
</script>

<p class="text-center text-4xl py-9 underline decoration-sky-500">Gerenciar Agendamentos</p>

<div class="container flex justify-around ">
    <Api url="agendamentos"></Api>
</div>