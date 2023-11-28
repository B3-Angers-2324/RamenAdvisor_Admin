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
        <h2>User id {id}</h2>
        <div id="submit">
            <button class="material-symbols-rounded icon" style="background-color:var(--danger);" title="Refuser">close</button>
            <button class="material-symbols-rounded icon" title="Accepter">done</button>
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
            <h3>Commentaires</h3>
            {#each Array(5) as _, i}
                <div id="commentairesContainer">
                    <div id="commentaire">
                        <div id="left">
                            <div id="title">
                                <h3>Restaurant {(Math.random() * 2 + 1).toFixed()}</h3>
                            </div>
                            <p>In deserunt quis proident adipisicing cillum velit incididunt reprehenderit ut dolor elit anim ipsum culpa. Veniam elit ut sint exercitation non sint. Cupidatat sunt dolore deserunt aliqua non.
                                In deserunt quis proident adipisicing cillum velit incididunt reprehenderit ut dolor elit anim ipsum culpa. Veniam elit ut sint exercitation non sint. Cupidatat sunt dolore deserunt aliqua non.
                                In deserunt quis proident adipisicing cillum velit incididunt reprehenderit ut dolor elit anim ipsum culpa. Veniam elit ut sint exercitation non sint. Cupidatat sunt dolore deserunt aliqua non.
                            </p>
                        </div>
                        <div id="right">
                            <p>{(Math.random() * 5).toFixed(1)}/5</p>
                            <button class="material-symbols-rounded">check</button>
                        </div>
                    </div>
                </div>
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

            #commentairesContainer{
                display: flex;
                justify-content: center;
                align-items: center;
                width: 100%;
                height: fit-content;
                margin-bottom: var(--spacing);

                #commentaire{
                    display: flex;
                    justify-content: space-between;
                    align-items: center;
                    width: 100%;
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
        }
    }
</style>