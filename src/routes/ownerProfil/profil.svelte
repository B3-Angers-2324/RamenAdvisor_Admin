<script lang="ts">
    import Template from "../../lib/template.svelte";
    import CustomInput from "../../lib/customInput.svelte";
    import { API_URL } from "../../main";
    import { onMount } from "svelte";
    import Modal from "../../lib/modal.svelte";

    let url = window.location.href;
    let id = url.substring(url.lastIndexOf('/') + 1);

    let information = {
        firstName: "",
        lastName: "",
        email: "",
        companyName: "",
        socialAdresse: "",
        siret: ""
    }
    let error = "";
    let showModal = false;

    onMount (async () => {
        fetchOwner(id);
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
        }else{
            console.log(data.message);
        }
    }

    async function validate () {
        let response = await fetch(`${API_URL}/admin/validate/${id}`, {
            method: "PATCH",
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + localStorage.getItem('token')
            }
        })
        let data = await response.json();
        if(response.ok){
            window.location.href = '/home';
        }else{
            console.log(data.message);
        }
    }

    async function handleDeleteRestaurant(){
        fetch(`${API_URL}/admin/deleteOwner/${id}`, {
            method: "delete",
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + localStorage.getItem('token')
            },
        })
        .then((res) => {
            window.location.href = '/home'
            res.json()
        })
        .then((data) => {
        })
    }
</script>

<Template>
    <h1>Owner Profil</h1>
    <div id="content">
        <div id="containerInput">
            <CustomInput title="Nom" type="text" bind:value={information["lastName"]} required/>
            <CustomInput title="Prénom" type="text" bind:value={information["firstName"]} required/>
            <CustomInput title="Email personnel" bind:value={information["email"]} type="email" required/>
            <CustomInput title="Nom de l'entreprise" type="text" bind:value={information["companyName"]} required/>
        </div>
        <div id="containerInput">
            <CustomInput title="Adresse du siège social" type="text" bind:value={information["socialAdresse"]} required/>
            <CustomInput title="Numéro de siret" type="text" bind:value={information["siret"]} required disabled/> <!-- il es disable, il ne doit pas être envoyé quand il modifie quelquechose -->
        </div>
        
    </div>
    <div id="submit">
        <!-- <button class="material-symbols-rounded icon" style="background-color:var(--danger);" title="Refuser">close</button> -->
        <button on:click={validate} class="material-symbols-rounded validate" title="Accepter">done</button>
            
        <button type="button" class="material-symbols-rounded delete" on:click={() => showModal = true}>delete</button>
    </div>
    <Modal bind:showModal validate={handleDeleteRestaurant}>
        <h2 slot="header">Voulez-vous vraiment supprimer cette owner ?</h2>
    </Modal>
</Template>

<style lang="scss">
    h1{
        color: var(--black);
        margin-bottom: 1em;
        padding: var(--spacing) 0 0 var(--spacing);
    }
    #content{
        display: flex;
        position: relative;
        width: 100%;
        flex-direction: row;

        #containerInput{
            display: flex;
            flex-direction: column;
            width: 50%;
            padding: var(--spacing);
        }
    }
    #submit{
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        margin: var(--spacing) 0;
        gap: var(--spacing);

        .validate, .delete{
            position: relative;
            cursor: pointer;
            padding: calc(var(--spacing) / 2) calc(var(--spacing) * 2);
            border-radius: var(--radius);
            border: none;
            color: var(--bone);
    
            &:focus{
                outline: none;
            }
        }

        .validate{
            background-color: var(--brunswick-green);
        }
        .delete{
            margin-left: 2em;
            background-color: var(--danger);
        }
    }
</style>