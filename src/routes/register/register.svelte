<script>

</script>

<main>
    <div id='block'>
    <h1> Sign in</h1>

    <form>
        <div class="form-group">
            <label for="email">Email *</label>
            <input class="form-control" id="email" name="email" type="email" placeholder="Email" required>
        </div>
        <div class="form-group">
            <label for="password">Password *</label>
            <input class="form-control" id="password" name="password" type="password" placeholder="Password" required>
        </div>
        <div class="form-group">
            <button class="btn" type="submit">Sign in</button>
        </div>
    </div>
</main>

<style lang="scss">
    main {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;

        #block{
            background-color: var(--zomp);
            padding: var(--spacing);
            border-radius: var(--radius);
            
            h1{
                margin-bottom: calc(var(--spacing) / 2);
            }
            form{
                width: 20em;
                .form-group{
                    display: flex;
                    flex-direction: column;
                    margin-bottom: calc(var(--spacing) / 2);
                    .form-control{
                        background-color: var(--brunswick-green);
                        border: none;
                        height: 3em;
                        border-radius: var(--radius);
                        padding-left: calc(var(--spacing) / 4);
                        &:focus{
                            outline: none;
                        }
                    }
                    .btn{
                        background-color: var(--brunswick-green);
                        border: none;
                        height: 3em;
                        border-radius: var(--radius);
                        color: var(--bone);
                        font-size: 1.1em;
                        cursor: pointer;
                    }
                }


            }
        }

    }
   


</style>