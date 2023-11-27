<script>
    import Radio from './Radio.svelte'
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
    var dadosJSform = []
	async function getEMP(){
        var dados = await fetch("http://localhost:8080/usuarios", {
            method: "GET",
            headers: {
            'Content-Type': 'application/json'
        }
        })
        dadosJSemp = await dados.json();
    }
    getEMP()   

	$: result = {
		name, length, type
	};
	
	$: errors = validate(touchedFields, result);

	const validate = () => {
		const errors = {};
		if (touchedFields.name && name === '') {
			errors.name = 'Name is required';
		}
		if (touchedFields.length && length == '') {
			errors.length = 'Digite o horario';
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
					dia: result.name,
					horario: result.length,
                    feito: radioValue,
					usuario: {
						id: result.type
					}
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
    const options = [{
                value: 1,
                label: 'feito',
            },{
                value: 0,
                label: 'Nao feito',
            }]
    let radioValue;
</script>


    <div class="bg-[#D4CDC5] p-10 rounded-2xl w-1/3">
            {#each dadosJS as dado}
                <p class="border-4 border-[#D4CDC5] border-b-black text-2xl p-1 flex justify-between">{dado.dia} <button class="bg-red-600 p-2 rounded-xl" on:click={delet(dado.id, url)}>Deletar</button></p>
            {/each}
    </div>

    <div class="form">
        <fieldset class="fieldset">
            <legend>Cadastrar</legend>
            <Input
                type="text"
                label="dia"
                bind:value={name}
                on:blur={() => touchedFields.name = true}
                error={errors.name}
            />
            <Input
                type="text"
                label="horario"
                bind:value={length}
                on:blur={() => touchedFields.length = true}
                error={errors.length}
            />
            
        
        
        <Radio {options} fontSize={16} legend='Feito' bind:userSelected={radioValue}/>

            <Select
                label="usuario"
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

<style>
    .formu{
        width: 35%;
        
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