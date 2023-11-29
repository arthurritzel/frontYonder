<script>
    export var url = ""
    export var dadosJS = []
    async function get(){
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
    
   
    async function delet(id, url){
        var dados = await fetch("http://localhost:8080/"+url+"/excluir/"+id, {
            method: "DELETE"
        })
        get()
    }

	import Input from '$lib/Input.svelte';
	import Button from '$lib/Button.svelte';
	
	export let data = {};
	export let onSubmit = () => {};

	let name = data.name ?? '';
	let length = data.length;
	let errors = {};
	let touchedFields = {};
	  
	$: result = {
		name, length
	};
	
	$: errors = validate(touchedFields, result);

	const validate = () => {
		const errors = {};
		if (touchedFields.name && name === '') {
			errors.name = 'Name is required';
		}
		if (touchedFields.length && length.length < 11) {
			errors.length = 'Digite um cnpj valido';
		}
		return errors;
	};
	
	const validateAndSubmit = () => {
		touchedFields = { name: true, length: true};
		if (!Object.keys(errors).length) {
			async function post(){
				var dados = await fetch("http://localhost:8080/"+url+"/adicionar", {
				method: "POST",
				body: JSON.stringify({
					nome: result.name,
					cnpj: result.length,
				}),
				headers: {
				'Content-Type': 'application/json'
				}
				
				})
				
                get()
			}
			post()
		}
	};
    var modal_verify = false
    var nome, cnpj
    function modal(id){
        modal_verify = true
        dadosJS.forEach(element => {
            if(element.id == id){
                nome = element.nome
                cnpj = element.cnpj
            }
        });
    }
</script>


    <div class="bg-[#D4CDC5] p-10 rounded-2xl w-1/2">
            {#each dadosJS as dado}
            <div class="border-4 border-[#D4CDC5] border-b-black itens">
                <p class=" text-2xl p-1 flex justify-between">{dado.nome}</p>
                <button class="bg-red-600 p-2 rounded-xl" on:click={delet(dado.id, url)}>Deletar</button>
                <button class="bg-blue-600 p-2 rounded-xl" on:click={modal(dado.id)}>Visualizar</button>
            </div>
            {/each}
    </div>

    <div class="formu">
        <fieldset class="fieldset"> 
            <legend>Cadastrar</legend>
            <Input
                type="text"
                label="Nome"
                bind:value={name}
                on:blur={() => touchedFields.name = true}
                error={errors.name}
            />
            <Input
                type="text"
                label="CNPJ"
                bind:value={length}
                on:blur={() => touchedFields.length = true}
                error={errors.length}
            />
            <Button on:click={validateAndSubmit}>Submit</Button>
             
        </fieldset>
    </div>

    {#if modal_verify}
        <div class="modal bg-white shadow dark:bg-gray-800">
            <p>Nome: {nome}</p>
            <p>CNPJ: {cnpj}</p>
            <br>
            <div>
                <button class="bg-red-600 p-2 rounded-xl" on:click={()=>{modal_verify = false}}>fechar</button>
            </div>
        </div>
        
    {/if}

<style>
    .itens{
        display: flex;
        margin-top: 15px;
    }
    .itens p {
        width: 80%;
    }
    .itens button{
        margin-left: 10px;
    }
    .modal{
        position: absolute;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
        
        margin-left: 200px;

        border-radius: 20px;
        padding: 10px;
        box-shadow: 0px 0px 10px 0px;
    }
    .modal p{
        font-size: 30px;
        margin: 10px;
        color: white;
    }
    .modal div{
        width: 90%;
        display: flex;
        justify-content: right;
    }
    .formu{
        width: 20%;
        
    }
    .fieldset > * {
        display: block;
    }
    
    .fieldset > :global(:not(legend) + *) {
        margin-top: 16px;
    }
    
    .fieldset {
        border: 1px solid #ddd;
        border-radius: 4px;
        padding: 20px;
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
</style>