<script>
import { createEventDispatcher } from "svelte";


    export let id, text, completed;

    const dispatch = createEventDispatcher();

    function triggerUpdate() {
        dispatch('update', { id, text, completed })
    }

    function handleDblclick() {
        // simple javascript prompt window
        const yes = confirm('Are you sure you wish to delete this item ') 

        if (yes) {
            dispatch("delete", id)
        }
    }

</script>

<div 
    class="item" 
    class:completed
    on:dblclick={handleDblclick}    
>

    <input 
        type="text" 
        class="text-input" 
        bind:value={text} 
        readonly={completed}
        on:keyup={( { key, target } ) => key === 'Enter' && target.blur() }
        on:blur={() => triggerUpdate()}    
    >
    <input 
        type="checkbox" 
        class="completed" 
        bind:checked={completed}
        on:change={() => triggerUpdate()}    
    >
</div>

<style>
    .item {
        display: flex;
        align-items: center;
        padding: 15px;
        background: white;
    }

    .item:focus-within {
        background: rgba(255, 255, 255, 0.8);
    }

    .text-input {
        flex-grow: 1;
        background: none;
        border: none;
        outline: none;
        font-weight: 500;
        font-size: 1em;
    }

    .completed {
        margin-left: 15px;
    }

    .item.completed {
        background: #ddd;
    }

    .item.completed .text-input {
        color: #555;
        text-decoration: line-through;
    }
</style>