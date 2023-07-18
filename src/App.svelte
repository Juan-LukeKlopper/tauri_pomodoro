<script>
  import Greet from './lib/Greet.svelte'
  import { tweened } from 'svelte/motion';
  let original = 5 * 3; // TYPE NUMBER OF SECONDS HERE
  let timer = tweened(original)
  let mode = "planning";
  let work_time_expired = false;

  setInterval(() => {
    if ($timer > 0) $timer--;
  }, 1000);

  function changeMode() {
    if (mode == "planning") {
        mode = "working"
    } else {
        mode = "planning"
    }
  }

  function resetTimer() {
      if ($timer <= 0) {
          work_time_expired = true;
          return false
      } else {
          work_time_expired = false;
          return true
      }
}

  $: minutes = Math.floor($timer / 60);
  $: minname = minutes > 1 ? "mins" : "min";
  $: seconds = Math.floor($timer - minutes * 60)
  $: fn = resetTimer(); 
</script>

<main class="container">
  <h1>Welcome to your personal tauri app!</h1>

  <div class="row">
    <Greet />
  </div>
   
  <p>{$timer/original}</p>
  <button class="row" on:click={changeMode}>
    lets GO!
  </button>


  {#if mode == "planning"}
    <input type="number" value={original} min="0" />Work for x minutes:
    <p>Rest for x minutes: </p>
  {:else}
    {#if fn}
        
    
    <h1>Your break starts in:  <span class="mins">{minutes}</span>{minname} 
	
	<span class="secs">{seconds}</span>s!</h1>

    <progress value={$timer/original}></progress>

    {:else}
    <h1>Enjoy your break <span class="mins">{minutes}</span>{minname} 
	
	<span class="secs">{seconds}</span>s!</h1>
    {/if}
  {/if}

</main>

