<script lang="ts">
    import Template from "../../lib/template.svelte";
    import CustomInput from "../../lib/customInput.svelte";
    import { API_URL } from "../../main";
    import { onMount } from "svelte";

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

    onMount (async () => {
        getOwner();
    })
    
    async function getOwner () {
        fetch(`${API_URL}/admin/getOne`, {
            method: "GET",
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + localStorage.getItem('token')
            }
        })
        .then((res) => res.json())
        .then((data) => {
            information = data.owner;
        })
    }

    const validate = () => {
        fetch(`${API_URL}/admin/validate`, {
            method: "PATCH",
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + localStorage.getItem('token')
            }
        })
        .then((res) => res.json())
        .then((data) => {
            error = data.messsage;
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
            <!-- <CustomInput title="Numéro de téléphone personnel" value="" type="tel" required/> -->
        </div>
        
    </div>
    <div id="submit">
        <!-- <button class="material-symbols-rounded icon" style="background-color:var(--danger);" title="Refuser">close</button> -->
        <button on:click={validate} class="material-symbols-rounded icon" title="Accepter">done</button>
    </div>
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

        .icon{
            position: relative;
            cursor: pointer;
            padding: calc(var(--spacing) / 2) calc(var(--spacing) * 2);
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
</style>