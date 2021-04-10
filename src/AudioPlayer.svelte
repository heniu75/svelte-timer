<script context="module">

    // script context of a module runs once for all the components of this type
    const players = new Set()
  
    export function stopAll() {
      players.forEach(p => p.pause())
    }
  </script>
  
  <script>
    import { onMount } from 'svelte'
  
    export let src;
    
    let player;

    export const play = () => {
      player.play();
    }
  
    onMount(() => {
      // Like players.push(player)
      players.add(player)
    })
  </script>
  
  <div>
      <audio
          bind:this={player}
          {src}
          controls>
          <track kind="captions" />
      </audio>
  </div>