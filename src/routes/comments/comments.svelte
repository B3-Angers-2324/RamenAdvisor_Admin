<script lang="ts">
    import Template from "../../lib/template.svelte";
    import { onMount } from "svelte";
    import { API_URL } from "../../main";

    let activeReport = []
    let noMoreReport = false;

    let restaurantName = "";

    onMount(async() => {
        loadReported(5, 0);
    });

    async function onRestaurantNameChange(e){
        e.preventDefault();
        activeReport = [];
        noMoreReport = false;
        loadReported(5, 0, restaurantName);
    }

    function loadReported(limit: number, offset: number, name: string = ""){
        let url : string = `${API_URL}/message/reported?limit=${limit}&offset=${offset}`;
        (name != "") ? url += `&restaurantName=${name}` : null;
        fetch(url ,{
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + localStorage.getItem('token')
            },
        })
            .then((res) => res.json() )
            .then(res => {
                activeReport = [...activeReport, ...res.obj];
                if(res.obj.length < limit){
                    noMoreReport = true;
                }
            })
            .catch(err => console.error(err));
    }

    function accept_message(reportId :string, index :number){
        if(deleteReport(reportId, false, index)){
            console.log("DeleteReport return true");
            activeReport.splice(index, 1);
            loadReported(1, 4);
        }
    }

    function reject_message(reportId :string, index :number){
        if(deleteReport(reportId, true, index)){
            console.log("DeleteReport return true");
            console.log(activeReport.splice(index, 1));
            loadReported(1, 4);
        }
    }

    async function deleteReport(reportId :string, rejected :boolean ,index :number){
        let output = false;
        let response = await fetch(`${API_URL}/message/report/${reportId}?rejected=${rejected}`,{
            method: 'DELETE',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + localStorage.getItem('token')
            },
        })
        if (response.status != 200) {
            alert("Erreur lors de l'interaction avec le signalement")
        }else{
            output = true;
        }
        return output;
    }
</script>

<Template>
    <h1>Commentaires</h1>

    <form on:submit={onRestaurantNameChange}>
        <input type="text" id="restaurant_name" name="restaurant_name" placeholder="Nom du restaurant" bind:value={restaurantName}>
    </form>

    {#each activeReport as report, i}
        <div class="commentairesContainer">
            <div class="commentaire">
                <div id="left">
                    <div id="title">
                        <h1>{report.user.firstName} {report.user.lastName}</h1>
                        <h1>Signalement: {report.nbReport}</h1>
                        <h3>{report.restaurant.name}</h3>
                    </div>
                    <p>{report.message.message}</p>
                </div>
                <div id="right">
                    <p>{report.message.note}/5</p>
                    <button class="material-symbols-rounded" on:click={()=>reject_message(report._id,i)}>close</button>
                    <button class="material-symbols-rounded" on:click={()=>accept_message(report._id,i)}>check</button>
                </div>
            </div>
        </div>
    {/each}
    {#if noMoreReport}
        <div class="commentairesContainer">No more report</div>
    {:else}
        <div class="commentairesContainer">. . .</div>
    {/if}
</Template>


<style lang="scss">
    form{
        width: 100%;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
        gap: var(--spacing);
        padding: 0 0 1em 0 ;

        input{
            margin: 0 var(--spacing);
            width: calc(100% - var(--spacing) * 2);
            height: 3em;
            padding: 0 calc(var(--spacing) / 2);
            border-radius: var(--radius);
            border: none;
            outline: none;
            font-size: 1em;
            color: var(--black);
        }
    }

    .commentairesContainer{
        display: flex;
        justify-content: center;
        align-items: center;
        width: 100%;
        height: fit-content;
        margin-bottom: var(--spacing);

        
        .commentaire{
            display: flex;
            justify-content: space-between;
            align-items: center;
            width: calc(100% - var(--spacing) * 2);
            height: 13em;
            background-color: var(--zomp);
            border-radius: var(--radius);
            
            #left{
                padding: calc(var(--spacing) / 2);
                display: flex;
                flex-direction: column;
                justify-content: space-between;
                align-items: flex-start;
                width: 75%;
                height: calc(100% - var(--spacing));

                #title{
                    display: flex;
                    width: 100%;
                    justify-content: space-between;
                    align-items: center;

                    h1,
                    h3{
                        color: var(--black);
                        padding: 0;
                        font-size: 1.5em;
                        font-weight: 600;
                        margin: 0;
                    }
                }

                p{
                    color: var(--black);
                    font-size: 1em;
                    max-height: 6em;
                    overflow: auto;
                }
            }

            #right{
                position: relative;
                display: flex;
                flex-direction: column;
                justify-content: center;
                align-items: center;
                width: 25%;
                height: 100%;
                background-color: var(--brunswick-green);
                border-radius: var(--radius);
                gap: 1em;

                p{
                    color: var(--bone);
                    font-size: 1.5em;
                }

                button{
                    position: absolute;
                    bottom: calc(var(--spacing) / 2);
                    right: calc(var(--spacing) / 2);
                    background: transparent;
                    border: none;
                    color: var(--bone);
                    font-size: 1.25em;
                    height: 3em;
                    width: 3em;
                    border-radius: 50%;
                    background-color: rgba(0, 0, 0, 0.15);
                    cursor: pointer;

                    &:nth-child(2){
                        left: calc(var(--spacing) / 2);
                    }
                }
            }
        }
    }
    
    h1{
        color: var(--black);
        margin-bottom: 1em;
        padding: var(--spacing) 0 0 var(--spacing);
    }
</style>