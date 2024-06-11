<script>
    import { onMount } from 'svelte';
  
    let output = null;
    let error = null;
  
    async function loadJSON(url) {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`Failed to load ${url}`);
      }
      return await response.json();
    }
  
    async function processJSON() {
      try {
        const inputA = await loadJSON('/inputA.json');
        const inputB = await loadJSON('/inputB.json');

        console.log(inputA, inputB)
  
        const inputAString = JSON.stringify(inputA);
        const inputBString = JSON.stringify(inputB);
  
        const result = window.processJSON(inputAString, inputBString);
  
        output = JSON.parse(result);

        console.log(output)
      } catch (err) {
        error = err.message;
      }
    }
  
    onMount(() => {
      processJSON();
    });
  </script>
  
  <main>
    {#if error}
      <p>Error: {error}</p>
    {/if}
    
    {#if output}
      <h1>Output:</h1>
      <p>Combined Name: {output.combined_name}</p>
      <p>Total Value: {output.total_value}</p>
      <p>Combined Details: {output.combined_details}</p>
    {/if}
  </main>
  
  <style>
    main {
      font-family: Arial, sans-serif;
      padding: 20px;
    }
  </style>
  