<script>
    let tg = window.Telegram.WebApp;
    let username = (tg.initDataUnsafe.user) ? tg.initDataUnsafe.user.first_name : "guest";
</script>

<main>
    <h1>
        Hello {username}
    </h1>
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
</style>
