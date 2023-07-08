<script lang="ts">
  import { DiscordSnowflake } from "@sapphire/snowflake";
  
  let date: number = Date.now();
  
  let firstNow = new Date(date); 
  
  let rightYear = firstNow.getFullYear();
  let rightMonth = firstNow.getMonth();
  let rightDay = firstNow.getDay();
  let rightHour = firstNow.getHours();
  let rightMinute = firstNow.getMinutes();
  let rightSecond = firstNow.getSeconds();
  
  $: year = rightYear;
  $: month = rightMonth - 1;
  $: day = rightDay;
  $: hours = rightHour;
  $: minutes = rightMinute;
  $: seconds = rightSecond;
  $: d = new Date(year, month, day, hours, minutes, seconds, firstNow.getMilliseconds()); 
  $: date = d.getTime();




  let testdate: any;

  let serverid: number = null;
  let channelid: number = null;

  $: uniqueId = DiscordSnowflake.generate({
    timestamp: new Date(!Number.isNaN(date) ? date : undefined),
  });
  $: link = `https://discord.com/channels/${serverid}/${channelid}/${uniqueId}`;

  const copy = async () => {
    try {
      await navigator.clipboard.writeText(link);
      console.log("Content copied to clipboard");
    } catch (err) {
      console.error("Failed to copy: ", err);
    }
  };

</script>

<main>
  <h3><label for="unix">Unix time in milliseconds:</label></h3>
  <br />
  <input
    type="number"
    name="unix"
    id="unixepoch"
    min="1"
    bind:value={date}
    required={true}
    placeholder="Fill me in milliseconds!" readonly={true}
  />
  ms
  <div class="inputLabel">Convert local YYYY / MM / DD</div>
  <input
    type="text"
    id="rightYear"
    class="conversionInput"
    bind:value={rightYear}
  />
  <input
    type="text"
    id="rightMonth"
    class="conversionInput"
    bind:value={rightMonth}
  />
  <input
    type="text"
    id="rightDay"
    class="conversionInput"
    bind:value={rightDay}
  />
  <div>
    <div class="inputLabel">and HH : MM : SS</div>
    <input
      type="text"
      id="rightHour"
      class="conversionInput"
      bind:value={rightHour}
    />
    <input
      type="text"
      id="rightMinute"
      class="conversionInput"
      bind:value={rightMinute}
    />
    <input
      type="text"
      id="rightSecond"
      class="conversionInput"
      bind:value={rightSecond}
    />
  </div>

  {date}
  <div>
    <br />

    {#if date === null || date === undefined}
      0
    {:else}
      {date}
      <br />
      snowflake: {uniqueId}
    {/if}

    <h2>generate fake discord message links to troll people with [BETA]</h2>

    <input
      type="text"
      name="serverid"
      id="serverid"
      required={true}
      bind:value={serverid}
      placeholder="server id"
    />
    <br />
    <input
      type="text"
      name="channelid"
      id="channelid"
      required={true}
      bind:value={channelid}
      placeholder="channel id"
    />

    <br />

    {#if serverid === null || channelid === null}
      Enter the values to generate :)
    {:else}
      <a href={link}>{link}</a>
      <br />
      <button on:click={copy}>Copy</button>
    {/if}
  </div>
</main>

<style>
</style>
