<script lang="ts">
    import Template from "../../lib/template.svelte";
    import CustomInput from "../../lib/customInput.svelte";

    let url = window.location.href;
    let id = url.substring(url.lastIndexOf('/') + 1);
</script>

<Template>
    <h1>Owner Profil {id}</h1>
    <div id="content">
        <div id="containerInput">
            <CustomInput title="Nom" type="text" value="" required/>
            <CustomInput title="Prénom" type="text" value="" required/>
            <CustomInput title="Email personnel" value="" type="email" required/>
            <CustomInput title="Nom de l'entreprise" type="text" value="" required/>
        </div>
        <div id="containerInput">
            <CustomInput title="Adresse du siège social" type="text" value="" required/>
            <CustomInput title="Numéro de siret" type="text" value="" required disabled/> <!-- il es disable, il ne doit pas être envoyé quand il modifie quelquechose -->
            <CustomInput title="Numéro de téléphone personnel" value="" type="tel" required/>
        </div>
        
    </div>
    <div id="submit">
        <button class="material-symbols-rounded icon" style="background-color:var(--danger);" title="Refuser">close</button>
        <button class="material-symbols-rounded icon" title="Accepter">done</button>
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