<script>
  let newHabit = '';
  let habits = JSON.parse(localStorage.getItem('habits') || '[]');

  function addHabit() {
    if (newHabit.trim() !== '') {
      habits = [...habits, { name: newHabit, done: false }];
      newHabit = '';
      saveHabits();
    }
  }

  function toggleHabit(index) {
    habits[index].done = !habits[index].done;
    saveHabits();
  }

  function deleteHabit(index) {
    habits.splice(index, 1);
    habits = [...habits]; // trigger reactivity
    saveHabits();
  }

  function resetHabits() {
    habits = habits.map(h => ({ ...h, done: false }));
    saveHabits();
  }

  function saveHabits() {
    localStorage.setItem('habits', JSON.stringify(habits));
  }
</script>

<main>
  <h1>Daily Habit Tracker</h1>
  <input bind:value={newHabit} placeholder="Enter a habit..." on:keydown={(e) => e.key === 'Enter' && addHabit()} />
  <button on:click={addHabit}>Add Habit</button>
  <button on:click={resetHabits}>Reset Today's Status</button>

  {#if habits.length > 0}
    <ul>
      {#each habits as habit, i}
        <li>
          <input type="checkbox" checked={habit.done} on:change={() => toggleHabit(i)} />
          <span class:done={habit.done}>{habit.name}</span>
          <button on:click={() => deleteHabit(i)} title="Delete Habit">❌</button>
        </li>
      {/each}
    </ul>
  {:else}
    <p>No habits yet. Start by adding one!</p>
  {/if}
</main>

<style>
  main {
    font-family: sans-serif;
    padding: 2rem;
    max-width: 500px;
    margin: auto;
  }
  input[type="text"] {
    width: 70%;
    padding: 0.5rem;
  }
  button {
    margin-left: 0.5rem;
    padding: 0.5rem;
  }
  ul {
    list-style: none;
    padding: 0;
    margin-top: 1rem;
  }
  li {
    margin: 0.5rem 0;
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }
  .done {
    text-decoration: line-through;
    color: gray;
  }
</style>
