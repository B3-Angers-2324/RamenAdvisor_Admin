<script>
    import Template from "../../lib/template.svelte";
    import { API_URL } from "../../main";
    let sanction = Number((Math.random() * 5).toFixed());


    let firstname
    let lastname

    let users = []

    async function searchUser(){
        console.log(firstname, lastname)
        //Build URL 
        let url = `${API_URL}/admin/search/user?`
        if(firstname) url += `firstName=${firstname}&`
        if(lastname) url += `lastName=${lastname}&`
        let response = await fetch(url, {
            method: "GET",
            headers: {
                'Authorization': 'Bearer ' + localStorage.getItem('token'),
                "Content-Type": "application/json"
            }
        });
        if (response.ok) {
            let data = await response.json();
            users = data.users
        } else {
            let data = await response.json();
            users = []
            console.log(data.message);
        }
    }
</script>

<Template>
    <h1>Ban User</h1>
    <div id="container">
        <form>
            <input bind:value={lastname} on:change={searchUser} type="text" id="name" name="name" placeholder="Nom">
            <input bind:value={firstname}  on:change={searchUser} type="text" id="name" name="name" placeholder="Prénom">
        </form>
        <div id="userContainer">
            {#each users as user}
                <a id="user" href="/banUser/user/{user._id}">
                    <h2>{user.lastName}</h2>
                    <span> - </span>
                    <h2>{user.firstName}</h2>
                    <h4>{user.email}</h4>
                </a>
            {/each}
            {#if users.length == 0}
                <h4>Aucun utilisateur trouvé</h4>
            {/if}
        </div>
    </div>
</Template>

<style lang="scss">
    h1{
        color: var(--black);
        margin-bottom: 1em;
        padding: var(--spacing) 0 0 var(--spacing);
    }

    #container{
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        width: calc(100% - var(--spacing) * 2);
        height: fit-content;
        padding: 0 var(--spacing);

        form{
            width: 100%;
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            align-items: center;
            gap: var(--spacing);

            input{
                width: 100%;
                height: 3em;
                padding: 0 calc(var(--spacing) / 2);
                border-radius: var(--radius);
                border: none;
                outline: none;
                font-size: 1em;
                color: var(--black);
            }
        }

        #userContainer{
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            width: 100%;
            padding: var(--spacing);
            gap: var(--spacing);

            #user{
                display: flex;
                align-items: center;
                width: calc(100% - var(--spacing));
                height: 4em;
                background-color: var(--zomp);
                border-radius: var(--radius);
                padding: 0 calc(var(--spacing) / 2);
                gap: var(--spacing);
                position: relative;

                h2{
                    color: var(--black);
                    font-size: 1em;
                }

                h4{
                    color: var(--black);
                    font-size: 1em;
                    position: absolute;
                    right: 0;
                    transform: translate(calc(-50% + var(--spacing) / 2), 0);
                }
            }
        }
    }
</style>