<script>
	import { onMount } from "svelte";
	import './button.scss';
  	import Button, {Group, GroupItem, Label, Icon} from '@smui/button';
	import ImageList, {Item, ImageAspectContainer, Image, Supporting} from '@smui/image-list';
	import './image-list.scss';

  let persons = [];
  let filter = "Alle";


  onMount(async function() {
	const response = await fetch(
	  "https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?content_type=person&include=0&select=sys.id,fields&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
	);
	persons = await response.json();
	persons =  persons.items;

  });
    const filterPersons = (filter, persons) =>
      filter === "Politiker"
        ? persons.filter(p => p.fields.category == "Politiker")
        : filter === "Entrepreneur"
        ? persons.filter(p => p.fields.category == "Entrepreneur")
        : persons;
</script>
<style>
	Image {
  		display: block;
  		max-width:230px;
  		max-height:95px;
  		width: auto;
  		height: auto;}
</style>

<svelte:head>
	<title>Gemeinsam lesen - gute Bücher von inspirierenden Personen empfohlen</title>
</svelte:head>

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
		<ImageAspectContainer>
		  <Image src={person["fields"]["pictureurl"]}></Image>
		</ImageAspectContainer>
		<Supporting>
		  <Label>{person["fields"]["name"]}</Label>
		</Supporting>
	  </Item>
	{/each}
  </ImageList>
</div>