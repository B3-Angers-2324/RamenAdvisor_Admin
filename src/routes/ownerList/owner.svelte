<script>
    import Template from "../../lib/template.svelte";
    import { Link } from "svelte-routing";

    let url = window.location.href;
    let id = url.substring(url.lastIndexOf('/') + 1);

    let nom = "Doe"
    let prenom = "Jhon"
    let mail = "john.doe@gmail.com"
    let birthDate = "01/01/2000"
</script>

<Template>
    <div id="top">
        <div id="icon">
            <Link to="/banUser/home">
                <span class="material-symbols-rounded">arrow_back</span>
            </Link>
        </div>
        <h2>Owner {id}</h2>
        <div id="submit">
            <button class="material-symbols-rounded icon" style="background-color:var(--danger);" title="Refuser">delete</button>
        </div>
    </div>
    <div id="content">
        <div id="info">
            <h3>Info</h3>
            <p><span>Name:</span> {nom}</p>
            <p><span>Prénom</span>: {prenom}</p>
            <p><span>Mail:</span> {mail}</p>
            <p><span>Date de naissance:</span> {birthDate}</p>
        </div>
        <div id="comments">
            <h3>Restaurants List</h3>
            {#each Array(5) as _, i}
                <a href="" id="restaurant">
                    <h2>Restaurant {i}</h2>
                </a>
            {/each}
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