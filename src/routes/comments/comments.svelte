<script lang="ts">
    import Template from "../../lib/template.svelte";
    import { onMount } from "svelte";
    import { API_URL } from "../../main";

    let activeReport = []
    let noMoreReport = false;

    onMount(() => {
        loadReported(5, 0);
    });

    function loadReported(limit: number, offset: number){
        fetch(`${API_URL}/message/reported?limit=${limit}&offset=${offset}`,{
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + localStorage.getItem('token')
            },
        })
            .then((res) => res.json() )
            .then(res => {
                console.log(res);
                activeReport = [...activeReport, ...res.obj];
                if(res.obj.length < limit){
                    noMoreReport = true;
                }
            })
            .catch(err => console.error(err));
    }

    function accept_message(reportId :string, index :number){
        if(deleteReport(reportId, false, index)){
            activeReport.splice(index, 1);
            loadReported(1, 4);
        }
    }

    function reject_message(reportId :string, index :number){
        if(deleteReport(reportId, true, index)){
            activeReport.splice(index, 1);
            loadReported(1, 4);
        }
    }

    function deleteReport(reportId :string, rejected :boolean ,index :number){
        let output = false;
        fetch(`${API_URL}/message/report/${reportId}?rejected=${rejected}`,{
            method: 'DELETE',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + localStorage.getItem('token')
            },
        })
            .then(async (res) => {
                console.log(res);
                let resJson = await res.json();
                return {status: res.status, body: resJson}
            })
            .then(res => {
                console.log(res);
                if (res.status != 200) alert(res.body.message)
                else {
                    output = true;
                }
            })
        return output;
    }
</script>

<Template>
    <h1>Commentaires</h1>

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
    {/if}
</Template>


<style lang="scss">
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