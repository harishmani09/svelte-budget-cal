<script>
import {onMount} from 'svelte';
let users = [];
let loading = true;

onMount( async() => {
    let userData = await fetch('https://api.github.com/users');
    let githubUsers = await userData.json();
    console.log(githubUsers);
    users = githubUsers;
    loading = false;
})

</script>

<style>
    h2 {
        text-align: center;
    }

</style>

{#if loading }
<h2> Loading...</h2>
{:else}
    <section>
        {#each users as user }
        <article class="user">
            <img src={user.avatar_url} alt={user.login} />
            <div class="user-info">
                <h4>{user.login}</h4>
                <a href="{user.html_url}" class="btn-primary" target="_blank">Github Profile</a>
            </div>

        </article>

        {/each}
    </section>
<h2>data</h2>
{/if}