<script>
	import AudioPlayer from './AudioPlayer.svelte';
	import Timer from './Timer.svelte';
	export let name;

	const src = 'assets/mixkit-scanning-sci-fi-alarm-905.wav';
    let audioPlayer;
	let myNode;

	const onTimerDone = ($event) => {
		console.log('timer done', $event.detail);
		console.log('ap', audioPlayer.play); //.play();
		audioPlayer.play();
		myNode.innerText = 'Heya';
	}

</script>

<main>
	<h1>{name}</h1>
	<Timer on:timerDone={onTimerDone}></Timer>
	<AudioPlayer {src} bind:this={audioPlayer}/>
	<div bind:this={myNode}>hello</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>