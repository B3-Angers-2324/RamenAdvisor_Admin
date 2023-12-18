<script>
    import Template from "../../lib/template.svelte";
    import { onMount } from "svelte";
    import { API_URL } from "../../main";

    let owners = [];

    onMount (async () => {
        getAllOwner();
    })

    async function getAllOwner () {
        fetch(`${API_URL}/admin/getValidate`, {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + localStorage.getItem('token')
            },
        })
        .then((res) => {
            if(res.status == 401){
                window.location.href = '/';
            }
            return res.json()
        })
        .then((data) => {
            owners = data.owners;
        })
    }
</script>

<Template>
    <h1>Owner List</h1>
    <div id="container">
        <form>
            <input type="text" id="name" name="name" placeholder="Nom">
            <input type="text" id="name" name="name" placeholder="Prénom">
        </form>
        <div id="userContainer">
            {#each owners as owner}
                <a id="user" href="/ownerList/owner/{owner._id}">
                    <h2>{owner.lastName}</h2>
                    <h2>{owner.firstName}</h2>
                </a>
            {/each}
            <!-- {#each Array(1) as _,i}
                <a id="user" href="/ownerList/owner/{i}">
                    <h2>Nom {i}</h2>
                    <h2>Prénom {i}</h2>
                </a>
            {/each} -->
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