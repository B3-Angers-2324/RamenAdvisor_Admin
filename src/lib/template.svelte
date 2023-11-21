<script lang="ts">
    import { API_URL } from "../main";

    import logo from '../assets/icon.png';

    let restaurants = [];

    // if(!localStorage.getItem('token')){
    //     window.location.href = '/';
    // }

    // fetch(`${API_URL}/owner/restaurants`, {
    //         method: 'GET',
    //         headers: {
    //             'Content-Type': 'application/json',
    //             'Authorization': 'Bearer ' + localStorage.getItem('token')
    //         },
    //     })
    //     .then((res) => {
    //         if(res.status == 401){
    //             window.location.href = '/';
    //         }
    //         return res.json()
    //     })
    //     .then((data) => {
    //         restaurants = data.restaurants;
    //     })

    const logout = () => {
        localStorage.removeItem('token');
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
        <div id="newOwnerContainer">
            <h3>
                <span class="material-symbols-rounded">list</span>
                Liste des Owners en attente
            </h3>
            <!-- la liste des commentaires -->
            <div id="newOwnerList">
                {#each Array(5) as _,i}
                    <a href='/profil/{i}' class="btn">
                        Owner name {i}
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

    main{
        align-items: center;
        height: 100vh;
        width: 100vw;
    }
    #popup{
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        padding: var(--spacing);
        border-radius: var(--radius);
        z-index: 100000;
        width: fit-content;
        height: fit-content;
        background-color: var(--bone);
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        gap: 1em;

        h1{
            color: var(--white);
            font-size: 2em;
            margin-bottom: 1em;
            display: flex;
            align-items: center;

            span{
                margin-right: calc(var(--spacing)/2);
                font-size: 1.5em;
            }
        }

        p{
            color: var(--white);
            font-size: 1.25em;
            margin-bottom: 1em;
        }

        #btn{
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 1em;

            a, button{
                padding: calc(var(--spacing) / 2) calc(var(--spacing) * 2);
                border-radius: var(--radius);
                border: none;
                background-color: var(--brunswick-green);
                color: var(--bone);
                cursor: pointer;
        
                &:focus{
                    outline: none;
                }
            }
        }
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
        #newOwnerContainer{
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

                .add{
                    width: 100%;
                    height: 2em;
                    background-color: var(--zomp);
                    border: none;
                    color: var(--bone);
                    font-size: 1.5em;
                    font-weight: bold;
                    cursor: pointer;
                    transition: .5s;
                    margin-bottom: 1em;
                    border-radius: var(--radius);
                    text-align: center;
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