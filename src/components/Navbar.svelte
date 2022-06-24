<script>

    import {onMount} from "svelte"

    let toggleMenu = false;

    const resetMenu = () => {toggleMenu = false}
    
    const toggleMenuFunc = () => {
        toggleMenu = !toggleMenu
    }

    onMount(() => {
        const mediaListener = window.matchMedia("(max-width: 650px)")

        mediaListener.addListener(resetMenu)
    })
</script>

<nav>
    <div on:click={toggleMenuFunc} class="logo">
        <div>&#9776;</div>
    </div>

    <ul class={toggleMenu ? "nav-list visible" : "nav-list"}>
        <li on:click={toggleMenuFunc} class="nav-item"><a href="/">Pokédex App Svelte</a></li>
    </ul>
</nav>

<style>
    nav {
        height: 70px;
        width: 100%;
        background:rgb(179,47,179);
        position: fixed;
        border-bottom: 1px solid #f1f1f1;
    }
    .logo {
        color: #fff;
        font-size: 40px;
        position: absolute;
        left: 10px;
        top: 5px;
        display: none;
    }
    .nav-list {
        z-index: 10;
        height: 100%;
        list-style-type: none;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .nav-list li a {
        color: #fff;
        text-decoration: none;
        margin: 0 10px;
        font-size: 22px;
    }
    @media only screen and (max-width: 650px) {
        .logo {
            display: block;
        }
        .nav-list {
            flex-direction: column;
            position: relative;
            top: 70px;
            display: none;
        }
        .nav-item {
            width: 100vw;
            background: rgb(179,47,179);
            text-align: center;
            padding: 10px 0;
        }
        .nav-list.visible {
            display: block;
        }
    }
</style>