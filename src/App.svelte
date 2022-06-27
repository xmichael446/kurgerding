<svelte:head>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
</svelte:head>

<script>
    import axios from 'axios';
    import {onMount} from 'svelte'

    let tg = window.Telegram.WebApp;
    let username = (tg.initDataUnsafe.user) ? tg.initDataUnsafe.user.first_name : "guest";

    let data = [];
    let payload = []
    onMount(async () => {
        axios.get("https://my-json-server.typicode.com/xmichael446/AlgorithmsInCpp/products")
            .then((res) => {
                data = res.data;
                payload = data.map(p => ({...p, count: 0}));
            })
    })

    const buy = (index) => {
        tg.MainButton.text = "Hello";
        tg.MainButton.enable()
        tg.MainButton.show();

        payload[index].count = 1;
        event.target.style.display = "none";

        document.getElementById(`${payload[index].name}-counter`).style.display = "inline-block"
    }

    const changeCount = (index, x) => {
        payload[index].count += x;

        if (payload[index].count <= 0) {
            payload[index].count = 0;

            document.getElementById(`${payload[index].name}-counter`).style.display = "none"
            document.getElementById(`${payload[index].name}-buy-btn`).style.display = "inline-block"
        }
    }

    const sendData = () => {
        let d = payload.filter((obj) => {
            console.log(obj)
            return obj.count > 0;
        })

        tg.sendData(JSON.stringify(d))
    }

    tg.MainButton.onClick(() => {
        sendData()
    })
</script>

<main>
    <h1 class="tg-elem">
        Hello {username}
    </h1>

    <div class="container">
        {#each data as product, i}
            <div class="card tg-elem">
                <div class="card-img">
                    <img src="{product.image}" alt="{product.name}">
                </div>
                <div class="card-body">
                    <div class="card-title">
                        {product.name}
                    </div>
                    <div class="card-text">
                        {product.description}
                    </div>
                </div>
                <div class="card-footer">
                    <button id="{product.name}-buy-btn" class="card-button" on:click={() => {buy(i)}}>
                        Buy
                    </button>
                    <div id="{product.name}-counter" class="counter">
                        <button class="counter-button" on:click={() => {changeCount(i, -1)}}>-</button>
                        <span>{payload[i].count}</span>
                        <button class="counter-button" on:click={() => {changeCount(i, 1)}}>+</button>
                    </div>
                </div>
            </div>
        {/each}
    </div>
    <div class="main_btn">
        <button class="order-button" on:click={sendData}>
            Order
        </button>
    </div>
</main>

<style>
    *,
    *::before,
    *::after {
        padding: 0;
        margin: 0;
        color: var(--tg-theme-text-color);
        background: var(--tg-theme-bg-color);
    }

    button {
        border-radius: 5px;
        border: 1px solid gray;
    }

    .tg-elem {
        color: var(--tg-theme-text-color);
        background: var(--tg-theme-bg-color);
    }

    .container {
        display: grid;
        grid-template-columns: 1fr 1fr;
        grid-gap: 0.5rem
    }

    .card {
        border: 1px solid gray;
        border-radius: 10px;
    }


    .card-img img {
        border-radius: 10px 10px 0 0;
        border-bottom: 1px solid gray;
    }

    .card-img img {
        width: 100%;
        height: auto;
    }

    .card-body {
        text-align: center;
        padding: 2px;
    }

    .card-title {
        font-weight: bolder;
    }

    .card-text {
        color: var(--tg-theme-hint-color);
        line-height: 1.1;
        padding: 0 10%;
    }

    .card-footer {
        text-align: center;
        border-top: 1px solid gray;
        padding: 5px;
    }

    .card-button {
        padding: 2px 20px;
    }

    .counter {
        display: none;
    }

    .counter-button {
        padding: 0 10px;
    }

    .main_btn {
        text-align: center;
        margin-top: 20px;
    }

    .order-button {
        font-size: 2rem;
        padding: 1rem 4rem;
        border-radius: 1rem;
    }
</style>
