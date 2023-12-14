<script lang="ts">
    import { get } from "svelte/store";
    import Template from "../lib/template.svelte";
    import { API_URL } from "../main";
    import { onMount } from "svelte";

    let foodtypes = [];
    let error = "";
    let inputText, inputFile;

    onMount(() => {
        getAllFoodTypes();
    })

    const getAllFoodTypes = () => {
        foodtypes = [];
        inputFile.value = "";
        inputText.value = "";
        fetch(`${API_URL}/foodtype`, {
                method: 'GET',
                headers: {
                    'Content-Type': 'application/json',
                },
            })
            .then((res) => res.json())
            .then((data) => {
                data.forEach((foodtype) => {
                    foodtypes = [
                    ...foodtypes,
                    {
                        _id: foodtype._id,
                        name: foodtype.name,
                        url: `${API_URL}/image/${foodtype.imgId}`
                    }
                ]
                })
            })
    }

    const handleSendFoodtype = (e) => {
        e.preventDefault();

        const apiRequest = API_URL + new URL(e.target.action).pathname;
        
        const formData = new FormData();
        formData.append('name', e.target.name.value);
        formData.append('image', e.target.svg.files[0]);
        
        fetch(apiRequest, {
                method: 'POST',
                headers: {
                    'Authorization': 'Bearer ' + localStorage.getItem('token')
                },
                body: formData
            })
            .then((res) => res.json())
            .then((data) => {
                getAllFoodTypes();
            })
    }

    const handleOnDelete = (id) => {
        fetch(`${API_URL}/foodtype/${id}`, {
                method: 'DELETE',
                headers: {
                    'Authorization': 'Bearer ' + localStorage.getItem('token')
                }
            })
            .then((res) => res.json())
            .then((data) => {
                getAllFoodTypes();
            })
    }
</script>

<Template>
    <h1>Create Foodtype</h1>
    <div id="container">
        <form action="/foodtype" on:submit|preventDefault={handleSendFoodtype}>
            <input type="text" name="name" id="" placeholder="Name of foodtype" bind:this={inputText}>
            <input name="svg" type="file" bind:this={inputFile}>
            <input type="submit">
            <p style="color: red">{error}</p>
        </form>
        <div id="containerImages">
            {#each foodtypes as foodtype}
                <div>
                    <h2>{foodtype.name}</h2>
                    <button on:click={() => handleOnDelete(foodtype._id)}>
                        <span class="material-symbols-rounded">delete</span>
                    </button>
                    <img src={foodtype.url} style="width:200px;" alt="">
                </div>
            {/each}
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
    padding: 0 var(--spacing);
    width: calc(100% - var(--spacing) * 2);
    height: 100%;

    form{
        width: 100%;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
        gap: var(--spacing);
        margin-bottom: calc(var(--spacing) / 2);
    
        input{
            width: 100%;
            height: 3em;
            padding: 0 calc(var(--spacing) / 2);
            border-radius: var(--radius);
            border: none;
            outline: none;
            font-size: 1em;
            color: var(--black);
    
            &[type="submit"]{
            width: 33%;
            height: 3em;
            padding: 0 calc(var(--spacing) / 2);
            border-radius: var(--radius);
            border: none;
            outline: none;
            font-size: 1em;
            color: var(--black);
            background-color: var(--zomp);
            cursor: pointer;
            }
        }

        input[type="file"]{
            height: fit-content;
            width: 100%;
        }
    }

    #containerImages {
        display: grid;
        width: 100%;
        grid-template-columns: repeat(auto-fill, minmax(15em, 1fr));
        gap: 20px;
   
        div {
            background-color: var(--zomp);
            width: 15em;
            height: 15em;
            position: relative;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            border-radius: var(--radius);
            
            img {
                width: 100%;
                height: 100%;
                object-fit:  fill;
            }
        }
    
    }


}

</style>