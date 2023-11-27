<button type="button" class="button" on:click>
	<slot />
</button>

<style>	
	.button {
		border-radius: 20px;
		border-color: #a0c0f0;
		background-color: #243A69;
		color: white;
		transition: .2s ease all;
		padding: 4px 20px;
		font-size: 30px;
	}

	.button:hover {
		border-color: #b0d0f0;
		background-color: #bdf;
		color: #243A69;
	}
</style>