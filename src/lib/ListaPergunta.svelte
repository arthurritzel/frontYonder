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
	import Select from '$lib/Select.svelte';
	import Button from '$lib/Button.svelte';
	
	export let data = {};
	export let onSubmit = () => {};

	let name = data.name ?? '';
	let length = data.length;
	let type = data.type ?? 'original';
	let errors = {};
	let touchedFields = {};

	var dadosJSemp = []
	
    dadosJSemp = [{
        id: 'writing',
        nome: 'writing'
    },
    {
        id: 'reading',
        nome: 'reading'
    },{
        id: 'speaking',
        nome: 'speaking'
    }]
    

	$: result = {
		name, length, type
	};
	
	$: errors = validate(touchedFields, result);

	const validate = () => {
		const errors = {};
		if (touchedFields.name && name === '') {
			errors.name = 'Name is required';
		}
		return errors;
	};
	
	const validateAndSubmit = () => {
		touchedFields = { name: true, length: true, type: true };
		if (!Object.keys(errors).length) {
			async function post(){
				var dados = await fetch("http://localhost:8080/"+url+"/adicionar", {
				method: "POST",
                
				body: JSON.stringify({
                    cabecalho: result.name,
                    tipo_prova: result.type,
                    nivel: result.length
					
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
    var cabecalho, tipo_prova, nivel
    function modal(id){
        modal_verify = true
        dadosJS.forEach(element => {
            if(element.id == id){
                cabecalho = element.cabecalho
                tipo_prova = element.tipo_prova
                nivel = element.nivel
            }
        });
    }
</script>


    <div class="bg-[#D4CDC5] p-10 rounded-2xl w-1/2">
            {#each dadosJS as dado}
            <div class="itens border-4 border-[#D4CDC5] border-b-black">
                <p class="text-2xl p-1 flex justify-between">{dado.cabecalho}</p> 
                <button class="bg-red-600 p-2 rounded-xl" on:click={delet(dado.id, url)}>Deletar</button>
                <button class="bg-blue-600 p-2 rounded-xl" on:click={modal(dado.id)}>Visualizar</button>

            </div>
            {/each}
    </div>

    <div class="form">
        <fieldset class="fieldset">
            <legend>Cadastrar</legend>
            <Input
                type="text"
                label="Cabecalho"
                bind:value={name}
                on:blur={() => touchedFields.name = true}
                error={errors.name}
            />
            <Input
                type="number"
                label="nivel"
                bind:value={length}
                on:blur={() => touchedFields.length = true}
                error={errors.length}
            />
            <Select
                label="tipo_prova"
                bind:value={type}
                on:blur={() => touchedFields.type = true}
                error={errors.type}
            >
                {#each dadosJSemp as dado}	
                    <option value="{dado.id}">{dado.nome}</option>
                {/each}
            </Select>
            
            <Button on:click={validateAndSubmit}>Submit</Button>
             
        </fieldset>
    </div>

    {#if modal_verify}
        <div class="modal bg-white shadow dark:bg-gray-800">
            <p>Cabeçalho: {cabecalho}</p>
            <p>Tipo Prova: {tipo_prova}</p>
            <p>Nivel: {nivel}</p>
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
    .form{
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