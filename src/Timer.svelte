<script>
  import { createEventDispatcher } from "svelte";
  import { writable } from "svelte/store";

  const dispatch = createEventDispatcher();

  let maxSeconds = 15; // seconds in half an hour;
  let counter = writable({
    totalSeconds: maxSeconds,
    running: false,
  });

  function onTimerDone() {
    dispatch("timerDone", {
      text: `Hello ${new Date()}!`,
    });
  }

  let status = "Click me to Start";
  let intervalHandle = null;

  const onClick = () => {
    console.log("starting countdown...");
    if (intervalHandle) {
      clearInterval(intervalHandle);
    } else {
      counter.update((value) => ({
        ...value,
        running: true,
      }));
      intervalHandle = setInterval(() => {
        console.log("inside interval", new Date());
        let newVal;
        counter.update((value) => {
          newVal = value.totalSeconds - 1;
          return {
            ...value,
            totalSeconds: newVal,
          };
        });
        if (newVal <= 0) {
          counter.update((value) => ({
            totalSeconds: maxSeconds,
            running: false,
          }));
          clearInterval(intervalHandle);
          onTimerDone();
        }
      }, 1000);
    }
  };

  $: seconds = Math.floor($counter.totalSeconds % 60).toString().padStart(2, "0");
  $: minutes = Math.floor($counter.totalSeconds / 60).toString().padStart(2, "0");
  $: status = $counter.running ? `${minutes}:${seconds}` : "Click me to start";

</script>

<button on:click={onClick}>{status}</button>
