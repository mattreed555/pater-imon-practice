<script>
  import LanguagePlayer from './components/LanguagePlayer.svelte';
import Header from './components/Header.svelte';

  import { data } from './data.js';
</script>
  
<div class="centered">
  <Header></Header>
  <LanguagePlayer data={data}></LanguagePlayer>
</div>

<style>

 body {
  background: linear-gradient(135deg, #FFFFF8 0%, #FFFFFB 100%);
  min-height: 100vh;
  margin: 0;
  display: flex;
  flex-direction: column;
  padding: 16px;
  box-sizing: border-box;
}
  
  	.centered {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
		justify-content: center;
		gap: 1em;
		max-width: 50em;
		margin: 0 auto;
		padding: 1em 0;
	}
  
 
</style>