<script>
    import { onMount } from 'svelte';
    import { items } from '../stores';
    import TodoApi from '../TodoApi'
    import Item from './Item.svelte';
    import NewItem from './NewItem.svelte';
    import { v4 as uuidv4 } from 'uuid'

    function handleNewItem(e) {
        // console.log(e)
        $items = [
            {
                id: uuidv4(),
                text: e.detail,
                completed: false
            },
            ...$items
        ];
        TodoApi.save($items)
    }

    function handleUpdate(e) {
        // console.log('updating items!')
        // console.log(e)
        const index = $items.findIndex( item => item.id === e.detail.id);
        $items[index] = e.detail;
        TodoApi.save($items);

    }

    function handleDelete(e) {
        // console.log(e)
        $items = $items.filter( item => item.id !== e.detail);
        TodoApi.save($items);

    }

    let itemsSorted = [];
    // reactive statement 
    $: {
        itemsSorted = [...$items];
        itemsSorted.sort((a, b) => {
            if(a.completed && b.completed) return 0;
            if(a.completed) return 1;
            if(b.completed) return -1;
        })
    }

    onMount( async() => {
        // fetch from API:
        $items = await TodoApi.getAll();
        // $items = []; // Make the items Array empty
    })
</script>

<div class="list">
    <NewItem on:newitem={handleNewItem} />
    {#each itemsSorted as item (item) }
        <!-- {JSON.stringify(item)} -->
        <Item {...item} on:update={handleUpdate} on:delete={handleDelete} />
        {:else}
        <p class="list-status">No Item Exist</p>
    {/each}
</div>

<style>
    .list {
        padding: 15px;
    }

    .list-status {
        margin: 0;
        text-align: center;
        color: white;
        font-weight: bold;
        font-size: 1.1em;
    }
</style>