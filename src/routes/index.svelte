
<!-- This is the landingpage-->
<script lang="ts">
	//onMount starts stuff before loading the page
	import { onMount } from "svelte";
	//importing styling from materialui
	import './button.scss';
  	import Button, {Label} from '@smui/button';
	import ImageList, {Item, ImageAspectContainer, Image, Supporting} from '@smui/image-list';
	import './image-list.scss';
	// import page information for base url
	import { stores } from '@sapper/app';
	const {page} = stores();
	

	//Variabled used in onMount thats why they are needed here - we could also typescript this here
  	let persons = [] ;
  	let filter = "Alle";

	
  onMount(async function() {
	//Start db connection with contentful
	const response = await fetch(
	  "https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?content_type=person&include=0&select=sys.id,fields&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
	);
	//transform response to json and store in array
	const persondata = await response.json();
	//.items is the part of the json that we need
	persons =  persondata.items;

  });
  	// now we filter persons based on the category, Multiple Categories 
    const filterPersons = (filter, persons) =>
      filter === "Politiker"
        ? persons.filter(p => p.fields.category == "Politiker")
        : filter === "Entrepreneur"
        ? persons.filter(p => p.fields.category == "Entrepreneur")
        : persons;
</script>
<style>
</style>

<svelte:head>
	<base href={$page.path} />
	<title>Gemeinsam lesen - gute Bücher von inspirierenden Personen empfohlen</title>
</svelte:head>

<!--Hardcoded because they dont change that often, should be changed to use chips instead of Buttons-->
<div class="filterCategories btn-group">
    <Button on:click={()=> filter = "Politiker"} ><Label>Politiker</Label></Button>
	<Button on:click={()=> filter = "Influencer"} ><Label>Influencer</Label></Button>
	<Button on:click={()=> filter = "Wissenschaftler"} ><Label>Wissenschaftler</Label></Button>
	<Button on:click={()=> filter = "Sportler"} ><Label>Sportler</Label></Button>
	<Button on:click={()=> filter = "Journalist"} ><Label>Journalist</Label></Button>
	<Button on:click={()=> filter = "Andere"} ><Label>Andere</Label></Button>
	<Button on:click={()=> filter = "Entrepreneur"} ><Label>Entrepreneur</Label></Button>
	<Button on:click={()=> filter = "Schauspieler" }><Label>Schauspieler</Label></Button>
	<Button on:click={()=> filter = "Künstler" }><Label>Künstler</Label></Button>
    <Button on:click={()=> filter = 'Alle'} ><Label>Alle</Label></Button>
</div>
  <div>
  <ImageList class="my-image-list-4x4" withTextProtection>
	{#each filterPersons(filter,persons) as person}
	<Item>
		<a href={"http://"+ $page.host + "/person/" + person["fields"]["name"]} src={person["fields"]["pictureurl"]} >
		<ImageAspectContainer>
			
			<Image src={person["fields"]["pictureurl"]}></Image>

		</ImageAspectContainer>
		<Supporting>
		  <Label>{person["fields"]["name"]}</Label>
		</Supporting>
		</a>
	  </Item>
	{/each}
  </ImageList>
</div>