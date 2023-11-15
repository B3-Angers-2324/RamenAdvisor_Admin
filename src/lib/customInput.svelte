<script>
    export let type = "";
    export let title = "".toLowerCase();
    export let required = false;
    export let handleExitInput = (e) => {};

    export let value = "";

    const handleChange = (e) => {
        switch(type){
            case "tel":
                if(e.target.value.length > 10)
                    e.target.value = e.target.value.slice(0, 10);
                else if(e.target.value.length > 0)
                    e.target.value = e.target.value.replace(/[^0-9]/g, '');
                break;
        }
        value = e.target.value;
    }

    const handleExit = (e) => {
        handleExitInput(e);
    }
</script>

<buttonCustom>
    {#if type=="submit"}
        <input type="{type}" value="{title}">
    {:else if type!="select"}
        <label for="{title}">{required ? `${title} *` : title}</label>
        <input type="{type}" name="{title}" id="{title}" placeholder="{title}" {required} value={value} on:focusout={handleExit} on:input={handleChange}>
    {:else}
        <label for="{title}">{required ? `${title} *` : title}</label>
        <select name="{title}" id="{title}" value={value} on:change={handleChange}>
            <option value="" disabled selected>Sélectionnez une option</option>
            <slot />
        </select>
    {/if}
</buttonCustom>

<style lang="scss">
    buttonCustom{
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

</style>