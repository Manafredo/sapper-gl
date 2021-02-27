<script>
	import { onMount } from "svelte";
	import './button.scss';
  import Button, {Group, GroupItem, Label, Icon} from '@smui/button';
  import './button.scss';

  let persons = [];

  onMount(async function() {
	const response = await fetch(
	  "https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?content_type=person&include=0&select=sys.id,fields&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
	);
	persons = await response.json();
	persons =  persons.items;
  });
  let filter = "all";
    const filterPersons = (filter, persons) =>
      filter === "Politiker"
        ? persons.filter(p => p.fields.category == "Politiker")
        : filter === "Entrepreneur"
        ? persons.filter(p => p.fields.category == "Entrepreneur")
        : persons;
</script>

<style>
	 p {
		text-align: center;
		margin: 1em auto;
	}
</style>

<svelte:head>
	<title>Gemeinsam lesen - gute Bücher von inspirierenden Personen empfohlen</title>
</svelte:head>

<div class="filterCategories btn-group stack-exception">
    <Button class="btn toggle-btn" aria-pressed={filter === "Politiker"} on:click={()=> filter = "Politiker"} >
      <span>Politiker</span>
    </Button>
	<Button class="btn toggle-btn" aria-pressed={filter === "Influencer"} on:click={()=> filter = "Influencer"} >
		<span>Influencer</span>
	</Button>
	<Button class="btn toggle-btn" aria-pressed={filter === "Wissenschaftler"} on:click={()=> filter = "Wissenschaftler"} >
		<span>Wissenschaftler</span>
	</Button>
	<Button class="btn toggle-btn" aria-pressed={filter === "Sportler"} on:click={()=> filter = "Sportler"} >
		<span>Sportler</span>
	</Button>
	<Button class="btn toggle-btn" aria-pressed={filter === "Journalist"} on:click={()=> filter = "Journalist"} >
		<span>Journalist</span>
	</Button>
	<Button class="btn toggle-btn" aria-pressed={filter === "Andere"} on:click={()=> filter = "Andere"} >
		<span>Andere</span>
	</Button>
    <Button class="btn toggle-btn"  aria-pressed={filter === "Entrepreneur"} on:click={()=> filter = "Entrepreneur"} >
      <span>Entrepreneur</span>
    </Button>
    <Button class="btn toggle-btn" aria-pressed={filter === 'Alle'} on:click={()=> filter = 'Alle'} >
      <span>Alle</span>
    </Button>
  </div>
  <ul>
    {#each filterPersons(filter,persons) as person}
        <p>{person["fields"]["name"]}</p>
        <p>{person["fields"]["description"]}</p>
    {/each}
  </ul>
