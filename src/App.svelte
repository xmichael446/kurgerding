<svelte:head>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css">
</svelte:head>

<script>
    import axios from 'axios';
    import {Button, Card, CardBody, CardImg, CardText, CardTitle, Col, Row} from "sveltestrap";

    let data = [];
    axios.get("https://my-json-server.typicode.com/xmichael446/AlgorithmsInCpp/products")
        .then((res) => {
            data = res.data;
        })
    let tg = window.Telegram.WebApp;
    let username = (tg.initDataUnsafe.user) ? tg.initDataUnsafe.user.first_name : "guest";
</script>

<main>
    <h1>
        Hello {username}
    </h1>
    <Row>
        {#each data as product}
            <Col sm="6" xs="6">
                <Card>
                    <CardImg src="{product.image}"/>
                    <CardBody class="tg-elem">
                        <CardTitle class="tg-text">
                            {product.name}
                        </CardTitle>
                        <CardText class="tg-text">
                            {product.description}
                        </CardText>
                        <Button class="tg-elem">Buy</Button>
                    </CardBody>
                </Card>
            </Col>
        {/each}
    </Row>
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

    .tg-elem {
        color: var(--tg-theme-text-color);
        background: var(--tg-theme-bg-color);
    }
</style>
