<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
  
    let fieldOfStudyOptions = [];
    let highestDegreeOptions = [];
    let demographicsOptions = [];
    let selectedFieldOfStudy = '';
    let selectedHighestDegree = '';
    let selectedDemographics = '';
    let data = [];
  
    onMount(async () => {
      const csvData = await d3.csv('/static/highered.csv');
      data = csvData;
      // Assuming your CSV has headers that match the filter names
      fieldOfStudyOptions = Array.from(new Set(csvData.map(d => d.fieldOfStudy)));
      highestDegreeOptions = Array.from(new Set(csvData.map(d => d.highestDegree)));
      demographicsOptions = Array.from(new Set(csvData.map(d => d.demographics)));
    });
  
    function filterData() {
      let filteredData = data;
      if (selectedFieldOfStudy) {
        filteredData = filteredData.filter(d => d.fieldOfStudy === selectedFieldOfStudy);
      }
      if (selectedHighestDegree) {
        filteredData = filteredData.filter(d => d.highestDegree === selectedHighestDegree);
      }
      if (selectedDemographics) {
        filteredData = filteredData.filter(d => d.demographics === selectedDemographics);
      }
      // Now you can pass this filteredData to your D3 chart rendering functions
    }
</script>

<div>
<select bind:value={selectedFieldOfStudy} on:change={filterData}>
    <option value="">Select Field of Study</option>
    {#each fieldOfStudyOptions as option}
    <option value={option}>{option}</option>
    {/each}
</select>

<select bind:value={selectedHighestDegree} on:change={filterData}>
    <option value="">Select Highest Degree</option>
    {#each highestDegreeOptions as option}
    <option value={option}>{option}</option>
    {/each}
</select>

<select bind:value={selectedDemographics} on:change={filterData}>
    <option value="">Select Demographics</option>
    {#each demographicsOptions as option}
    <option value={option}>{option}</option>
    {/each}
</select>
</div>

<!-- Here you will add your D3 visualization containers -->
<div id="barPlot"></div>
<div id="histogram"></div>

<style>
/* Add your styles here */
</style>


