
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
	import Chip, {Set, Icon, Text} from '@smui/chips';
	let choice = "Politiker";
	let personcategories =["Politiker","Influencer","Wissenschaftler","Sportler","Journalist","Entrepreneur"]
	

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
	// if elly is bored she could create a loop out of this to make it even more elegant
    const filterPersons = (filter, persons) =>
      filter === "Politiker"
        ? persons.filter(p => p.fields.category == "Politiker")
        : filter === "Entrepreneur"
        ? persons.filter(p => p.fields.category == "Entrepreneur")
		: filter === "Influencer"
        ? persons.filter(p => p.fields.category == "Influencer")
		: filter === "Wissenschaftler"
        ? persons.filter(p => p.fields.category == "Wissenschaftler")
		: filter === "Sportler"
        ? persons.filter(p => p.fields.category == "Sportler")
		: filter === "Journalist"
        ? persons.filter(p => p.fields.category == "Journalist")
        : persons;
</script>
<style>
</style>

<svelte:head>
	<base href={$page.path} />
	<title>Gemeinsam lesen - gute Bücher von inspirierenden Personen empfohlen</title>
</svelte:head>

<!--Hardcoded because they dont change that often, should be changed to use chips instead of Buttons-->

<div>
    <Set chips={personcategories} let:chip choice bind:selected={choice}>
      <Chip><Text>{chip}</Text></Chip>
    </Set>
  </div>



  <div>
  <ImageList class="my-image-list-4x4" withTextProtection>
	{#each filterPersons(choice,persons) as person}
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
{#each filterPersons(choice,persons) as person}
	<div style="visibility: hidden; position: absolute">
		<a href={"http://"+ $page.host + "/person/" + person["fields"]["name"]}>.</a>
	</div>
{/each}