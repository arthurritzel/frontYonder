<script>
    import Logo from "$lib/Logo.svelte"
</script>

<div>
    <h1>Projeto Yonder</h1>
    <p>Manipulação de API</p>
    <div class="logos">
        <img src="logoYonder.png" alt="logo yonder" class="logo">
        <img src="spring_logo.png" alt="">
        <img src="svelte_logo.png" alt="">
    </div>
</div>

<style>
    div{
        width: 100%;
        display: flex;
        align-items: center;
        flex-direction: column;
    }
    h1{
        font-size: 50px;
        margin-top: 50px;
        background-color: rgba(7, 7, 66, 0.68);
        color: rgb(255, 255, 255);
        padding: 10px;
        border-radius: 20px;
    }

    p{
        font-size: 30px;
    }
    img{
        margin-top: 50px;
        width: 30%;
    }
    .logos{
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: space-around;
        width: 100%;
    }
</style>