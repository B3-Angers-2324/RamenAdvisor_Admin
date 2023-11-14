<script>

</script>

<div class="mainContainer">
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div id="container">
        <h1>Sign In</h1>
        <form>
            <div class="input">
                <label for="email">Email *</label>
                <input type="email" id="email" name="email" placeholder="Email" required>
            </div>

            <div class="input">
                <label for="password">Password *</label>
                <input type="password" id="password" name="password" placeholder="Password" required>
            </div>

            <div class="input">
                <input type="submit" value="Sign In">
            </div>
        </form>
    </div>
</div>

<style lang="scss">
    .mainContainer{
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: center;
    }
    :global(main){
        width: 100%;
        height: 100vh;
        
        #container{
            background-color: var(--zomp);
            width: fit-content;
            height: fit-content;
            padding: var(--spacing);
            border-radius: var(--radius);
            margin: auto;
            

            h1{
                color: var(--black);
                margin-bottom: calc(var(--spacing) / 2);
            }

            form{
                display: flex;
                flex-direction: column;
                width: 20em;
                margin-bottom: var(--spacing);

                .input{
                    display: flex;
                    flex-direction: column;
                    width: 100%;
                    
                    label{
                        color: var(--black);
                        margin-bottom: .5em;
                        user-select: none;
                    }
                    
                    input, select{
                        margin-bottom: calc(var(--spacing) / 2);
                        padding: calc(var(--spacing) / 4);
                        border-radius: var(--radius);
                        border: none;
                        background-color: var(--brunswick-green);
                        color: var(--bone);
                
                        &:focus{
                            outline: none;
                        }
                    }

                    input[type="submit"]{
                        cursor: pointer;
                    }

                    /* Chrome, Safari, Edge, Opera */
                    input::-webkit-outer-spin-button,
                    input::-webkit-inner-spin-button {
                    -webkit-appearance: none;
                    margin: 0;
                    }

                    /* Firefox */
                    input[type=number] {
                        -moz-appearance: textfield;
                    }
                }
            }

            a{
                color: var(--black);
                text-decoration: none;
                font-size: .8em;
                letter-spacing: .1em;
                cursor: pointer;

                &:focus{
                    outline: none;
                }
            
            }
        }
    }

    @media only screen and (max-width: 600px){
        :global(main){
            #container{
                width: 100vw;
                padding: var(--spacing) 0;
                border-radius: 0;

                h1{
                    padding: 0 calc(var(--spacing) / 2);
                }

                form{
                    padding: 0 calc(var(--spacing) / 2);
                }
            }
        }
    }
</style>