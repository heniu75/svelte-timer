<script>
import { writable } from "svelte/store";

let maxSeconds = 30 * 60; // seconds in half an hour;
let counter = writable({
        totalSeconds: maxSeconds
    });

let status = 'Click me to Start';
let intervalHandle = null;

const onClick = () => {
    console.log('you clicked me');
    if (intervalHandle) {
        clearInterval(intervalHandle);
    } else {
        intervalHandle = setInterval(() => {
            console.log('inside interval', new Date);
            counter.update(value => ({ 
                totalSeconds: value.totalSeconds - 1
            }));
        }, 1000);
        status = 'Running'
    }
}

$: seconds = Math.floor($counter.totalSeconds % 60).toString().padStart(2,'0');
$: minutes = Math.floor($counter.totalSeconds / 60).toString().padStart(2,'0');

</script>

<button on:click={onClick}>{status} {minutes}:{seconds}</button>