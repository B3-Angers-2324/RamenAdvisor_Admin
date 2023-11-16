<script lang="ts">
    import Template from "../lib/template.svelte";
    import { API_URL } from "../main";

    let foodtypes = [];
    let error = "";

    async function loadImg(nameUrl){
        try{
            const response = await fetch(`${API_URL}/foodtype/${nameUrl}`, {
                    method: 'GET',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': 'Bearer ' + localStorage.getItem('token')
                    },
                })
            const name = response.headers.get('X-Name');
            const blob = await response.blob();
            const url = URL.createObjectURL(blob);

            console.log(name, url)

            foodtypes = [
                ...foodtypes,
                {
                    name: name,
                    url: url
                }
            ]
        }catch(err){
            console.log(err);
        }
    }

    fetch(`${API_URL}/foodtype`, {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
            },
        })
        .then((res) => res.json())
        .then((data) => {
            data.forEach((foodtype) => {
                loadImg(foodtype.name);
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
        
        // let b64;
        // let reader = new FileReader();
        // reader.readAsDataURL(e.target.svg.files[0]);
        // reader.onload = function () {
        //     b64 = btoa((reader.result as string));

        //     let data = {
        //         name: e.target.name.value,
        //         svg: e.target.svg.file[0],
        //     }
            
        //     fetch(apiRequest, {
        //             method: 'POST',
        //             headers: {
        //                 'Content-Type': 'application/json',
        //                 'Authorization': 'Bearer ' + localStorage.getItem('token')
        //             },
        //             body: JSON.stringify(data)
        //         })
        //         .then((res) => res.json())
        //         .then((data) => {
        //         })
        // };
        // reader.onerror = function (error) {
        //     console.log("Error while reading file");
        // };
    
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