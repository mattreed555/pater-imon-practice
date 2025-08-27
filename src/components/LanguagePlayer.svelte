<script>
  import { onMount } from 'svelte';
  
	export let data = {};
  let src = data.mp3Url;
  let lines = data.lines;
  
  
  
  let showHints = true;
	let time = 0;
	let duration = 0;
	let paused = true;
  let speed = "0.75"; 
  
	function format(time) {
		if (isNaN(time)) return '...';

		const minutes = Math.floor(time / 60);
		const seconds = Math.floor(time % 60);

		return `${minutes}:${seconds < 10 ? `0${seconds}` : seconds}`;
	}
  
 
  function updateSpeed() {
    let vid = document.getElementById("audioPlayer");
    vid.playbackRate = parseFloat(speed);
  }
  
  const onSpeedChange = () => {
		updateSpeed();
    
	}
  
  onMount(() => {
    let firstTime = data.lines[0].words[0].startTime;
    time = firstTime;
    updateSpeed();
  });
  

</script>

<div class="player" class:paused>
	<audio id="audioPlayer"
		{src}
		bind:currentTime={time}
		bind:duration
		bind:paused
		preload="metadata"
		on:ended={() => {
			time = 0;
		}}
	/>
	<div class="controls">
		<div class="control-group">
			<button
				class="play input"
				aria-label={paused ? 'play' : 'pause'}
				on:click={() => paused = !paused}
			/>
			<select class="input" name="Playback Speed" id="playback-speed" bind:value={speed} on:change={onSpeedChange}>
				<option value="0.75">Slow</option>
				<option value="1.6">Faster</option>
			</select>
		</div>
		<div class="control-group">
			<input class="input" type="checkbox" id="show-pronunciation" name="Show Pronunciation" value="Show Pronunciation Hints" bind:checked={showHints} >
			<label class="text" for="show-pronunciation">Show pronunciation hints</label>
		</div>
	</div>
    <br>
	<div class="info">
    <div>
    {#each lines as line}
      <div class="wrapper">
       {#each line.words as words}
        <div class="word">
          <button class="word" 
                  class:word-highlighted = {time > words.startTime && time <= (words.startTime + words.length)}
                  on:click={() => {
              time = words.startTime;
              paused = false;
            }}
            >{words.word}</button>
          {#if showHints}
          <label for="button">{words.pronunciation}</label>
            {:else}
            <label for="button">&nbsp;</label>
          {/if}
        </div>
       {/each}
      </div>
      <br>
    {/each}
    </div>  
	</div>
</div>

<style>
  
  @font-face {
    font-family: GFSDidot;
    src: url(../../assets/GFSDidot-Regular.ttf);
  }
  
  @font-face {
    font-family: OpenSans;
    src: url(../../assets/OpenSans-Regular.ttf);
  }



  
	.player {
		align-items: center;
		padding: 1em 1em 0.5em 1em;
		max-width: 50em;
		margin: 0 auto;
		box-shadow: 0 4px 16px rgba(0,0,0,0.08);
		border-radius: 12px;
		background: linear-gradient(135deg, #FFFFF8 0%, #FFFFFB 100%);
		box-sizing: border-box;
	}

  .controls {
		background-color: #D8DAD9;
		padding: 20px 24px;
		display: flex;
    align-items: center;
    justify-content: space-between;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    margin-bottom: 16px;
    gap: 20px;
	}

	.control-group {
		display: flex;
		align-items: center;
		gap: 12px;
	}
  
  	.input  {
    accent-color: #0F8A64;
		margin-left: 20px;
	}

	select.input {
		border: 2px solid #D8DAD9;
		border-radius: 6px;
		padding: 8px 12px;
		font-family: OpenSans, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
		font-size: 16px;
		background-color: white;
		color: #444;
		cursor: pointer;
		transition: all 0.2s ease;
		appearance: none;
		background-image: url('data:image/svg+xml;charset=US-ASCII,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 4 5"><path fill="%23666" d="M2 0L0 2h4zm0 5L0 3h4z"/></svg>');
		background-repeat: no-repeat;
		background-position: right 12px center;
		background-size: 12px;
		padding-right: 36px;
	}

	select.input:hover {
		border-color: #0F8A64;
		box-shadow: 0 2px 4px rgba(15,138,100,0.2);
	}

	select.input:focus {
		outline: none;
		border-color: #0F8A64;
		box-shadow: 0 0 0 3px rgba(15,138,100,0.1);
	}

	input.input[type="checkbox"] {
		height:18px;
		width:18px;
		margin-right: 12px;
		border-radius: 4px;
		border: 2px solid #D8DAD9;
		transition: all 0.2s ease;
		cursor: pointer;
	}

	input.input[type="checkbox"]:hover {
		border-color: #0F8A64;
		box-shadow: 0 2px 4px rgba(15,138,100,0.2);
	}

	.text {
		font-size:16px;
    font-family: OpenSans, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
		color:#444;
		font-weight: 500;
	}

  div.wrapper {
    display: flex;
    margin-bottom: 6px;
    padding: 4px 0;
  }
  
  div.word {
    display: grid;
    align-items: center;
    justify-content: center;
    margin: 0 4px;
  }
  
  label {
    font-family:OpenSans;
    font-size: 18px;
    text-align:center;
    color: #616161;
    text-shadow: 0 1px 2px rgba(0,0,0,0.1);
    line-height: 1.4;
  }
  
  button {
    cursor:pointer;
    transition: all 0.2s ease;
  }
  
  button.word {
    font-family: GFSDidot;
    font-size: 24px;
    align-items: normal;
    color: #313131;
    font-weight: normal;
    background-color: rgba(0,0,0,0);
    border-color: rgb(0, 0, 238);
    border-style: none;
    box-sizing: content-box;
    border-radius: 3px;
    margin: 2px;
    padding: 4px 8px;
    line-height: 1.6;
    text-shadow: 0 1px 2px rgba(0,0,0,0.05);
    transition: all 0.2s ease;
  }
  
  button.word-highlighted {
    font-weight: bold;
    background-color: rgba(220, 40, 40, 0.1);
    border-radius: 4px;
    transform: scale(1.02);
    box-shadow: 0 2px 4px rgba(220, 40, 40, 0.2);
  }
  
  button.word:hover {
    background-color: rgba(220, 40, 40, 0.05);
    transform: scale(1.05);
    box-shadow: 0 2px 4px rgba(0,0,0,0.15);
    border-radius: 4px;
  }
  
  @media screen and (max-device-width: 480px) 
    and (orientation: portrait) {
      
      .text {
       font-size: 10px;
      }
    button.word {
     font-size: 16px;
    }
      label {
        font-size: 12px;
      }
      
      input.input {
        font-size: 12px;
      }
}
  
  
	button.play {
		width: 2.5em;
		aspect-ratio: 1;
		background-repeat: no-repeat;
		background-position: 50% 50%;
		border-radius: 50%;
    stroke:#000000;
    background-color:#0F8A64;
    box-shadow: 0 2px 6px rgba(15,138,100,0.3);
    transition: all 0.2s ease;
	}

	button.play:hover {
		transform: scale(1.05);
		box-shadow: 0 4px 8px rgba(15,138,100,0.4);
		background-color:#11976F;
	}
	
	[aria-label="pause"] {
		background-image: url(/src/pause.svg);
	}

	[aria-label="play"] {
		background-image: url(/src/play.svg);
	}

	.info {
		overflow: hidden;
	}


</style>