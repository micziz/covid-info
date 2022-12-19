<script lang="ts">
  let country;
  let submitted = false;
  
  async function getData(country) {
    let data = await fetch(`https://disease.sh/v3/covid-19/countries/${country}?strict=true`)
    let json = await data.json()
    return json;
  }

  let promise;

</script>

<main>

  <h1>Data for country</h1>
  
  <div id="form">
    <form on:submit|preventDefault={() => {
      promise = getData(country)
      submitted = true;
    }}>
      <input id="formItself" type="text" bind:value={country}>
    </form>
  </div>
  
  {#if submitted}
    {#await promise}
      <h1>Loading</h1>
    {:then data} 
      <h1>{data.country}</h1>
  
      <div id="images">
        <img src={data.countryInfo.flag} alt="" srcset="">
      </div>
  
      <h1>Untill now, there have been {data.cases} cases in {data.country}</h1>
    {/await}
  {/if}
</main>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

  main{
    font-family: 'Poppins', sans-serif;
  }

  h1 {
    text-align: center;
  }

  #images{
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  #form{
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  #formItself{
    width: 24rem;
    font-size: 14pt;
  }


</style>