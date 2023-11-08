<script>
    export var id = -1
    export var table = ""
    var dadosJSdelet = []
    async function delet(){
        var dados = await fetch("http://localhost:8080/"+table+"/excluir/"+id, {
            method: "DELETE"
        })
        dadosJSdelet = await dados.json();
    
    }
</script>

<button class="bg-red-600 p-2 rounded-xl" on:click={delet}>Deletar</button>