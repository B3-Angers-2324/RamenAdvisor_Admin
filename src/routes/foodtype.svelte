<script lang="ts">
    import { get } from "svelte/store";
    import Template from "../lib/template.svelte";
    import { API_URL } from "../main";
    import { onMount } from "svelte";

    let foodtypes = [];
    let error = "";
    let inputText, inputFile;
    
    if(localStorage.getItem('ad') !== '1'){
        window.location.href = '/home';
    }

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

        // test if the image is a svg
        if (inputFile.files[0].type !== "image/svg+xml") {
            error = "The image must be a svg";
            return;
        }

        // test if the image is under 15Mo
        if (inputFile.files[0].size > 15000000) {
            error = "The image must be under 15Mo";
            return;
        }

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
            .then((res) => {
                if (res.status === 200) {
                    getAllFoodTypes();
                } else {
                    return res.json();
                }
            })
            .then((data) => {
                if (data) {
                    error = data.message;
                }
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
            <input name="svg" id="file" type="file" bind:this={inputFile} class="input-file" />
            <label for="file" class="label-file">
                <span class="material-symbols-rounded" style="padding-right: 10px;">draft</span>
                Choose a file
            </label>
            <input type="submit">
        </form>
        <p style="color: red">{error}</p>
        <div id="containerImages">
            {#each foodtypes as foodtype}
                <div>
                    <div id="header">
                        <h2>{foodtype.name}</h2>
                        <button on:click={() => handleOnDelete(foodtype._id)}>
                            <span class="material-symbols-rounded">delete</span>
                        </button>
                    </div>
                    <img src={foodtype.url} alt="">
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

        .label-file {
            border-radius: var(--radius);
            border: none;
            outline: none;
            font-size: 1em;
            color: var(--black);
            background-color: var(--zomp);
            cursor: pointer;
            width: 20em;
            height: fit-content;
            height: 3em;
            padding: 0 calc(var(--spacing) / 2);
            display: flex;
            justify-content: center;
            align-items: center;

        }

        .input-file {
            display: none;
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

            #header {
                height: 33%;
                width: 100%;
                display: flex;
                flex-direction: row;
                justify-content: space-around;
                align-items: center;

                h2 {
                    color: var(--black);
                    font-size: 1.5em;
                }

                button {
                    background-color: transparent;
                    border: none;
                    outline: none;
                    cursor: pointer;
                    transition: .5s;
                    width: 3em;
                    height: 3em;
                    display: flex;
                    justify-content: center;
                    align-items: center;
                    border-radius: 50%;

                    &:hover {
                        background-color: rgba(255, 255, 255, 0.1);
                    }

                }
            }
            
            img {
                width: 60%;
                height: 60%;
                object-fit:  fill;
            }
        }
    
    }


}

</style>