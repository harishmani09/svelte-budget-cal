<script>
    import {getContext} from 'svelte';
    import {blur, slide, scale,fade, fly } from 'svelte/transition';

    export let id;
    export let amount;
    export let name;
    let displayAmount = false;
    // export let removeExpense;
    // function toggleAmount(){
    //     displayAmount = !displayAmount;
    // }
    const removeExpense = getContext('remove');
    const setModifiedAmount = getContext('modify');
</script>

<article class="single-expense">

    <div class="expense-info">
        <h2> 
        {name} 
        <button class="amount-btn" on:click="{() => {displayAmount = !displayAmount}}">
            <i class="fas fa-caret-down"/>
        </button>
    </h2>

        {#if displayAmount}

        <h4 transition:fly={ {x:-100, y:-100, duration:1000, delay:100}}> Amount:Rs{amount}  </h4>
        {/if}
    </div>
        <div class="expense-buttons">
            <button class="expense-btn edit-btn" on:click="{() =>setModifiedAmount(id)}">
                <i class="fa fa-pen" />
            </button>
            <button class="expense-btn delete-btn" on:click={removeExpense(id)}>
                <i class="fa fa-trash" />
            </button>
            <!-- <button class="expense-btn delete-btn" on:click={() => dispatch('delete', {id, name:"hello, from object"})}>
                <i class="fa fa-trash" />
            </button> -->
        </div>


</article>