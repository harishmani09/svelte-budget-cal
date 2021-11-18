<script>
async function getUsers() {
    let userData = await fetch('https://api.github.com/users');
    let githubUsers = await userData.json();
    return githubUsers;

}
</script>

<section>
{#await getUsers()}    
<!--promise is pending-->
{:then users}

{#each users as user }
        <article class="user">
            <img src={user.avatar_url} alt={user.login} />
            <div class="user-info">
                <h4>{user.login}</h4>
                <a href="{user.html_url}" class="btn-primary" target="_blank">Github Profile</a>
            </div>

        </article>

        {/each}
        {:catch error}
        <p>something went wrong: {error.messages}</p>
{/await}
</section>