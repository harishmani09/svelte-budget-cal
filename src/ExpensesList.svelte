<script>
import {getContext} from 'svelte';
import {fly} from 'svelte/transition';
import SectionTitle from './Title.svelte';
import {flip} from 'svelte/animate';
import Expense from './Expense.svelte'
export let expenses = [];


const clearExpense = getContext('clear');

</script>

<section>
<SectionTitle title="expense list" />
<ul>
{#each expenses as expense, index (expense.id) }
    <div in:fly={{x:200, delay:(index+1)*700}} out:fly={{x:-200}} animate:flip >
    <Expense {...expense}  />
</div>
    {:else }
    <h3> currently you have no expenses</h3>

{/each}
</ul>

<button type="button" class="btn btn-primary btn-block" on:click={clearExpense} >clear expense</button>

</section>