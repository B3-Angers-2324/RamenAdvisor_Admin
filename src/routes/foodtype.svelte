<script lang="ts">
    import Template from "../lib/template.svelte";
    import { API_URL } from "../main";

    let foodtypes = [];
    let error = "";

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
                    name: foodtype.name,
                    url: `${API_URL}/image/${foodtype.imgId}`
                }
            ]
            })
        })

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
                console.log(data);
            })
    }
</script>

<Template>
    <form action="/foodtype" on:submit|preventDefault={handleSendFoodtype}>
        <input type="text" name="name" id="" placeholder="Name of foodtype">
        <input name="svg" type="file">
        <input type="submit">
        <p style="color: red">{error}</p>
    </form>
    {#each foodtypes as foodtype}
        <div>
            <h2>{foodtype.name}</h2>
            <img src={foodtype.url} style="width:200px;" alt="">
        </div>
    {/each}
</Template>

<style lang="scss">

</style>