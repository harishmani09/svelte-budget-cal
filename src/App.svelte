<script>
import {setContext, afterUpdate} from 'svelte';
import {getContext} from 'svelte';
// import Github from './Github.svelte';
// import Githubawait from './Githubawait.svelte';

//components
import Navbar from './Navbar.svelte';
import Title from './Title.svelte';
import ExpensesList from './ExpensesList.svelte';
import Totals from './Totals.svelte';
import ExpenseForm from './ExpenseForm.svelte';
import Modal from './Modal.svelte';

//data 
// import expensesData from './expenses';
import { onMount } from 'svelte';

//variables
// let expenses = [...expensesData];
let expenses = [];
//editing variables
let setName = '';
let setAmount= null;
let setId = null;

//toggle form variable
let isFormOpen = false;


//reactive
$: isEditing = setId ? true: false;
$: total= expenses.reduce( (acc,curr) => {    
    return acc += curr.amount},0)


//functions 

function showForm(){
    isFormOpen = true;
}
function hideForm(){
    isFormOpen = false;
    setName = '';
    setAmount = null;
    setId = null;
}

function removeExpense(id){
    expenses = expenses.filter( (item) => item.id !== id )
    
}
function clearExpense(){
    expenses = [];
    
}

function addExpense({name,amount}){
    let expense = {id: Math.random() * Date.now(), name, amount}
    expenses = [expense, ...expenses];
    
}

function setModifiedExpense(id){
    let expense = expenses.find( (item) => item.id === id)
    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
    showForm();
}

function editExpense({name,amount}){
    expenses = expenses.map( item => {
        return item.id === setId ? {...item, name, amount}: {...item}
    });
    setId = null;
    setAmount = null;
    setName = "";
    
}


//context
setContext('remove', removeExpense );
setContext('clear', clearExpense)
setContext('modify', setModifiedExpense);

//local storage
function setLocalStorage(){
    localStorage.setItem('expenses', JSON.stringify(expenses))
}

onMount( () => {
    expenses = localStorage.getItem('expenses') ? JSON.parse(localStorage.getItem('expenses')) :[];
});

afterUpdate( () => {
    setLocalStorage();
})

// function deleteExpense(event){
//     const {id,name} =event.detail;
//     removeExpense(id);
//     console.log(name);

// }

</script>
<Navbar {showForm}  />

<main class="content">
<!-- <Github /> -->
<!-- <Githubawait /> -->
{#if isFormOpen}    
<Modal>
<ExpenseForm {addExpense} name={setName} amount={setAmount} {isEditing} {editExpense} {hideForm}/>
</Modal>
{/if}
<Totals title="total expenses" {total} />
<ExpensesList {expenses} {removeExpense} />

</main>

