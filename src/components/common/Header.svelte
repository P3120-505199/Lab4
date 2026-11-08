<script>
    import { link, router } from "svelte-spa-router";

    import { logout } from "../../services/authService.js";
    import { currentUser, isAdmin, isAuthenticated } from "../../stores/authStore.js";

    const roleLabels = {
        ADMIN: "Администратор",
        USER: "Пользователь"
    };

    const onStudents = $derived(
        router.location === "/" || router.location.startsWith("/students")
    );

    const onProfile = $derived(router.location === "/profile");
</script>

<header>
    <a class="brand" href="/" use:link>Управление студентами</a>

    <nav>
        {#if $isAuthenticated}
            <a href="/" use:link aria-current={onStudents ? "page" : undefined}>Студенты</a>

            {#if $isAdmin}
                <a href="/students/new" use:link>Добавить студента</a>
            {/if}
        {/if}
    </nav>

    <div class="user">
        {#if $isAuthenticated && $currentUser}
            <a
                class="profile"
                href="/profile"
                use:link
                aria-current={onProfile ? "page" : undefined}
            >
                {$currentUser.username}
                <span class="role">{roleLabels[$currentUser.role] ?? $currentUser.role}</span>
            </a>

            <button type="button" onclick={logout}>Выйти</button>
        {:else}
            <a href="/login" use:link>Войти</a>
            <a href="/register" use:link>Регистрация</a>
        {/if}
    </div>
</header>

<style>
    header {
        display: flex;
        flex-wrap: wrap;
        align-items: center;
        gap: 1rem 2rem;
        margin-bottom: 2rem;
        padding-bottom: 1rem;
        border-bottom: 1px solid #cccccc;
    }

    .brand {
        font-size: 1.25rem;
        font-weight: bold;
    }

    nav {
        display: flex;
        gap: 1rem;
        flex: 1;
    }

    .user {
        display: flex;
        align-items: center;
        gap: 1rem;
    }

    a {
        color: #0645ad;
        text-decoration: none;
    }

    a:hover {
        text-decoration: underline;
    }

    a[aria-current="page"] {
        font-weight: bold;
        text-decoration: underline;
    }

    .brand {
        color: #000000;
    }

    .role {
        margin-left: 0.5rem;
        padding: 0.125rem 0.5rem;
        border: 1px solid #cccccc;
        border-radius: 0.75rem;
        background-color: #f2f2f2;
        color: #333333;
        font-size: 0.8rem;
        font-weight: normal;
    }

    button {
        padding: 0.375rem 0.75rem;
        border: 1px solid #cccccc;
        border-radius: 0.375rem;
        background-color: #eeeeee;
        cursor: pointer;
    }
</style>
