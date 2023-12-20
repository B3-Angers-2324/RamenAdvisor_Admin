<script>
    import Template from "../../lib/template.svelte";
    import { Link } from "svelte-routing";
    import { onMount } from "svelte";
    import { API_URL } from "../../main";

    let url = window.location.href;
    let id = url.substring(url.lastIndexOf('/') + 1);
    
    if(localStorage.getItem('ad') !== '1'){
        window.location.href = '/home';
    }

    let information = {
        firstName: "",
        lastName: "",
        email: "", 
        ban: ""
    }
    // let birthDate = ""
    let restaurants = [];
    let error = "";

    onMount (async () => {
        fetchOwner(id);
        getRestaurants();
    })

    async function fetchOwner(id){
        let response = await fetch(`${API_URL}/admin/owner/profile/${id}`,{
            method: "GET",
            headers: {
                'Authorization': 'Bearer ' + localStorage.getItem('token'),
                "Content-Type": "application/json"
            }
        })
        let data = await response.json();
        if(response.ok){
            information = data;
            updateBan();
        }else{
            console.log(data.message);
        }
    }

    async function getRestaurants () {
        fetch(`${API_URL}/admin/restaurants/${id}`, {
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
            restaurants = data.restaurants;
        })
    }

    async function banOwner() {
        let response = await fetch(`${API_URL}/admin/ban/${id}`,{
            method: "PATCH",
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + localStorage.getItem('token')
            },
            body: JSON.stringify({
                id: id,
                ban: true
            })
        })
        let data = await response.json();
        if (response.ok) {
            updateBan();
            window.location.href = "/ownerList/home";
        } else {
            console.log(data.message);
            alert("Une erreur est survenue")
        }
    }

    function updateBan() {
        let banButton = document.getElementById("banButton");
        let textStateBan = document.getElementById("textBan");
        if (information.ban) {
            banButton.style.display = 'none';
            textStateBan.style.display = 'block';
        }
        else {
            textStateBan.style.display = 'none';
        }
    }

    function deleteOwner() {
        fetch(`${API_URL}/admin/deleteOwner/${id}`, {
            method: "delete",
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + localStorage.getItem('token')
            },
        })
        .then((res) => {
            window.location.href = '/ownerList/home'
            res.json()
        })
        .then((data) => {
        })
    }
</script>

<Template>
    <div id="top">
        <div id="icon">
            <Link to="/ownerList/home">
                <span class="material-symbols-rounded">arrow_back</span>
            </Link>
        </div>
        <h2>Owner: {information.lastName} {information.firstName}</h2>
        <div id="submit">
            <button on:click={banOwner} class="material-symbols-rounded icon" style="background-color:var(--danger);" title="Ban" id="banButton">close</button>
            <button on:click={deleteOwner} class="material-symbols-rounded icon" style="background-color:var(--danger);" title="Delete">delete</button>
        </div>
    </div>
    <div id="content">
        <div id="info">
            <h3>Info</h3>
            <p><span>Name:</span> {information.lastName}</p>
            <p><span>Prénom:</span> {information.firstName}</p>
            <p><span>Mail:</span> {information.email}</p>
        </div>
        <div id="comments">
            <h3>Restaurants List</h3>
            {#each restaurants as restaurant}
                <a href={""} class="restaurant">
                    <h2>{restaurant.name}</h2>
                </a>
            {/each}
        </div>
        <div>
            &nbsp;
            <p id="textBan">L'utilisateur est ban</p>
        </div>
    </div>
</Template>

<style lang="scss">
    #top{
        display: flex;
        justify-content: flex-start;
        align-items: center;
        width: calc(100% - var(--spacing) * 2);
        height: fit-content;
        padding: var(--spacing);
        position: relative;

        h2{
            color: var(--black);
            margin-left: var(--spacing);
        }

        #icon{
            display: flex;
            justify-content: center;
            align-items: center;
            width: fit-content;
        }

        #submit{
            position: absolute;
            width: fit-content;
            display: flex;
            justify-content: center;
            align-items: center;
            gap: calc(var(--spacing) / 3);
            right: var(--spacing);

            .icon{
                position: relative;
                cursor: pointer;
                padding: calc(var(--spacing) / 3);
                border-radius: var(--radius);
                border: none;
                background-color: var(--brunswick-green);
                color: var(--bone);
                font-size: 2.5em;
        
                &:focus{
                    outline: none;
                }
            }
        }
    }

    #content{
        display: flex;
        justify-content: space-between;
        align-items: flex-start;
        width: calc(100% - var(--spacing) * 2);
        height: fit-content;
        margin-bottom: var(--spacing);
        padding: 0 var(--spacing);
        flex-direction: column;

        #info{
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
            align-items: flex-start;
            width: 30%;
            height: fit-content;

            h3{
                color: var(--black);
                margin-bottom: 1em;
            }

            p{
                color: var(--black);
                margin-bottom: .5em;

                span{
                    font-weight: 600;
                }
            }
        }

        #comments{
            width: 100%;
            margin-top: var(--spacing);

            h3{
                color: var(--black);
                margin-bottom: 1em;
            }

            #restaurant{
                display: flex;
                align-items: center;
                width: calc(100% - var(--spacing));
                height: 4em;
                background-color: var(--zomp);
                border-radius: var(--radius);
                padding: 0 calc(var(--spacing) / 2);
                gap: var(--spacing);
                position: relative;
                margin-bottom: calc(var(--spacing) / 2);

                h2{
                    color: var(--black);
                    font-size: 1em;
                }
            }
        }
    }
</style>