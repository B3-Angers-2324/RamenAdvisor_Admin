<script lang="ts">
    import { API_URL } from "../main";
    import logo from '../assets/icon.png';
    import { onMount } from "svelte";

    let admin = localStorage.getItem('ad');

    // exemple of the middleware of front
    fetch(`${API_URL}/admin/nav`, {
            method: "GET",
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + localStorage.getItem('token')
            }
        })
        .then((res) => res.json())
        .then((data) => {
            if(data.ad){
                localStorage.setItem('ad', '1');
                admin = '1';
            }

            // TODO : add menu here
            
        });
    let owners = [];

    onMount (async () => {
        getOwnerNoValidate();
    })

    async function getOwnerNoValidate () {
        fetch(`${API_URL}/admin/getValidate`, {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer ' + localStorage.getItem('token')
            },
        })
        .then((res) => {
            if(res.status == 401){
                window.location.href = '/';
            }
            return res.json()
        })
        .then((data) => {
            owners = data.owners;
        })
    }

    const logout = () => {
        localStorage.removeItem('token');
        localStorage.removeItem('ad');
        window.location.href = '/';
    }
</script>

<div class="template">
    <!-- La barre de nav à gauche -->
    <div id="nav">
        <!-- Le head -->
        <div id="head">
            <img src={logo} alt="logo">
            <h1>RamenAdvisor</h1>
        </div>
        {#if admin == '1'}
            <div id="createModContainer">
                <h3>
                    <span class="material-symbols-rounded">person_add</span>
                    Create Moderator
                </h3>
                <div id="foodTypeBtn">
                    <a href='/createModerator' class="btn">
                        Create
                    </a>
                </div>
            </div>
            <div id="foodTypeContainer">
                <h3>
                    <span class="material-symbols-rounded">edit</span>
                    Nourriture
                </h3>
                <div id="foodTypeBtn">
                    <a href='/foodtype' class="btn">
                        Editer type nouriture
                    </a>
                </div>
            </div>
        {/if}
        <div id="newOwnerContainer">
            <h3>
                <span class="material-symbols-rounded">list</span>
                Liste des Owners en attente
            </h3>
            <!-- la liste des commentaires -->


            <div id="newOwnerList">
                {#if owners.length == 0}
                    <p class="btn disable">Aucun owner en attente</p>
                {/if}
                {#each owners as owner}
                    <a href='/profil/{owner._id}' class="btn">
                        {owner.lastName + " " + owner.firstName}
                    </a>
                {/each}
            </div>


        </div>
        <div id="foodTypeContainer">
            <h3>
                <span class="material-symbols-rounded">chat</span>
                Commentaires Signalés
            </h3>
            <div id="foodTypeBtn">
                <a href='/comments' class="btn">
                    Commentaires
                </a>
            </div>
        </div>
        <div id="banUserContainer">
            <h3>
                <span class="material-symbols-rounded">gavel</span>
                Ban User
            </h3>
            <div id="foodTypeBtn">
                <a href='/banUser/home' class="btn">
                    Users
                </a>
            </div>
        </div>
        {#if admin == '1'}
            <div id="ownerList">
                <h3>
                    <span class="material-symbols-rounded">list</span>
                    Owner List
                </h3>
                <div id="foodTypeBtn">
                    <a href='/ownerList/home' class="btn">
                        List
                    </a>
                </div>
            </div>
        {/if}
        <div id="logout">
            <button on:click={logout}>
                <span class="material-symbols-rounded">logout</span>
                Disconnect
            </button>
        </div>
    </div>
    
    <div id="content">
        <slot></slot>
    </div>
</div>

<style lang="scss">    

    .template{
        width: 100%;
        height: 100vh;
        overflow: hidden;
        display: flex;
    }

    #nav{
        width: 25%;
        height: 100%;
        background-color: var(--bone);
        overflow: auto;

        #head{
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 20%;
            width: calc(100% - var(--spacing));
            background-color: var(--dark-bone);
            padding: calc(var(--spacing)/2);
            border-bottom: 2px solid var(--black);

            img{
                width: 7em;
            }

            h1{
                color: var(--white);
            }
        }

        #foodTypeContainer,
        #newOwnerContainer,
        #banUserContainer,
        #ownerList,
        #createModContainer{
            display: flex;
            flex-direction: column;
            justify-content: center;
            width: calc(100% - var(--spacing));
            background-color: var(--dark-bone);
            padding: calc(var(--spacing)/2);
            border-bottom: 2px solid var(--black);

            h3{
                color: var(--black);
                display: flex;
                align-items: center;
                padding-bottom: 1em;

                span{
                    margin-right: calc(var(--spacing)/2);
                    font-size: 2em;
                }
            }

            #foodTypeBtn,
            #newOwnerList{
                display: flex;
                flex-direction: column;
                justify-content: center;
                align-items: center;
                height: 100%;
                width: 100%;

                .btn{
                    width: 90%;
                    min-height: 3em;
                    background-color: var(--brunswick-green);
                    border: none;
                    color: var(--bone);
                    font-size: 1em;
                    font-weight: bold;
                    text-align: left;
                    padding: 0 calc(var(--spacing)/2);
                    cursor: pointer;
                    transition: .5s;
                    margin-bottom: 1em;
                    border-radius: var(--radius);
                    display: flex;
                    justify-content: space-between;
                    align-items: center;

                    &:hover{
                        transform: translateX(.5em);
                    }

                    &.disable{
                        background-color: var(--zomp);
                        color: var(--grey);
                        cursor: not-allowed;
                        transform: none;
                    }

                    a, button{
                        background: transparent;
                        border: none;
                        color: var(--bone);
                        font-size: 1.25em;
                        height: 1.5em;
                        width: 1.5em;
                        border-radius: 50%;
                        background-color: rgba(0, 0, 0, 0.15);
                        cursor: pointer;
                    }
                }
            }
        }

        #logout{
            display: flex;
            justify-content: center;
            align-items: center;
            width: calc(100% - var(--spacing));
            background-color: var(--dark-bone);
            padding: calc(var(--spacing)/2);
            gap: 1em;

            a, button{
                width: 100%;
                padding: calc(var(--spacing) / 2) 0;
                background-color: var(--zomp);
                border: none;
                color: var(--bone);
                font-weight: bold;
                font-size: 1em;
                cursor: pointer;
                transition: .5s;
                border-radius: var(--radius);
                display: flex;
                justify-content: center;
                align-items: center;

                span{
                    font-size: 1.5em;
                }
            }

            a{
                flex: 3;
                span{
                    padding: 0 calc(var(--spacing) / 2);
                }
            }
        }
    }

    #content{
        width: 75%;
        height: 100vh;
        overflow: auto;
    }
</style>