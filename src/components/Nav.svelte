<!-- This component will be loaded into every single page, this is very useful for stuff like navbars of footers -->

<!-- This Javascript piece  -->
<script lang="ts">

/*value gets loaded from index.svelte*/

  import { onMount } from "svelte";
  import { personstore } from '../routes/stores.js';

  let personfilter;
  const unsubscribe = personstore.subscribe(value => {
	personfilter = value;
  })
 
  import "@fortawesome/fontawesome-free/js/brands.min.js"
  import { stores } from '@sapper/app';
	const {page} = stores();
	import Textfield from '@smui/textfield';
	import List, { Item, Text, PrimaryText, SecondaryText} from '@smui/list';
	let searchTerm = '';
	let persons = [];



	onMount(async function() {
	//Start db connection with contentful
	const response = await fetch(
	  "https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?content_type=person&include=0&select=sys.id,fields&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
	);
	//transform response to json and store in array
	const persondata = await response.json();
	//.items is the part of the json that we need
	persons =  persondata.items;
	$personstore = persons;
	});

  // Loop through all list items, and hide those who don't match the search query
  const searchPersons = (search, persons) =>
    search.length >= 3
        ? persons.filter(p => p.fields.name.includes(search,0))
	:search === "Alle"
        ? persons.filter(p => p.fields.name.includes(" ",0))
		: "nix";
  
</script>

<!-- This is the css part with  sapper / svelte standard styling we took from the example  -->
<!-- Here we should try to implement the colors from Meltems Design Colourcoding -->
<style>
	nav {
		background-color: #EFEFEF;
		border-bottom: 1px solid #B1B1B1;
		font-weight: 300;
	}
	ul {
		margin: 0;
		padding: 0;
		justify-content: space-between;
	}
	/* clearfix */
	ul::after {
		content: '';
		display: block;
		clear: both;
		justify-content: space-between;
		
	}
	li {
		display: block;
		float: left;
	}
	
	a {
		text-decoration: none;
		padding: 1em 0.5em;
		display: block;
	}
  * :global(.shaped) {
    border-radius: 16px 16px 0 0;
  }
  * :global(.shaped-outlined .mdc-text-field__input) {
    padding-left: 32px;
    padding-right: 32px;
  }
  * :global(.shaped-outlined .mdc-notched-outline .mdc-notched-outline__leading) {
    border-radius: 28px 0 0 28px;
    width: 28px;
  }
  * :global(.shaped-outlined .mdc-notched-outline .mdc-notched-outline__trailing) {
    border-radius: 0 28px 28px 0;
  }
  * :global(.shaped-outlined .mdc-notched-outline .mdc-notched-outline__notch) {
    max-width: calc(100% - 28px * 2);
  }
  * :global(.shaped-outlined + .mdc-text-field-helper-line) {
    padding-left: 32px;
    padding-right: 28px;
  }
	img {
		text-decoration: none;
		display: block;
	}
	.flex-box {
		display:flex;
        justify-content:space-between;
		align-items: center;
        }

.dropdown-content {
  display:inherit;
  position:absolute;
  background-color: #f9f9f9;
  min-width:260px;
  z-index: 1;
}

</style>

<!-- This is the html part where we add the navbar -->
<!-- Navbar is added using material ui components that are loaded in he script section - there is no linking yet-->
<nav>
	<ul>
		<div class="flex-box">
			<div class="flex-item">
				<li><a href={"http://"+ $page.host + ""}><img src="logo_lang.png" alt="Logo" style="height:55px;"></a></li>
			</div>
			<div class="flex-item">
				<li>      
					<div>
						<Textfield class="shaped-outlined" variant="outlined" withLeadingIcon bind:value={searchTerm} label="Suche" style="height: 50%">
						</Textfield>
						{#if searchPersons(searchTerm, personfilter)=="nix"}
						<p style="display:none;">text eingeben</p>
						{:else}
						<div class="dropdown-content">
							<List class="demo-list">
								{#each searchPersons(searchTerm, personfilter) as item}
								<Item>
									<div style="display: flex; padding: 0 1em 0 0;">
										<a sapper:prefetch href={"/person/" + item["fields"]["name"]} target="_blank" src={item["fields"]["pictureurl"]} >
										<img src={item["fields"]["pictureurl"]} alt="url" style="border-radius: 50%; max-width: 35px;">
									</div>
									<div>
										<Text>
											<a href={"/person/" + item["fields"]["name"]} target="_blank" src={item["fields"]["pictureurl"]} >
											<PrimaryText>{item["fields"]["name"]}</PrimaryText>
											<SecondaryText>{item["fields"]["category"]}</SecondaryText>
										</Text>
									</div>
								</Item>
								{/each}
							</List>
						</div>
						{/if}
					</div>
				</li>
			</div>
			<div class="flex-item">
				<li style="float: right; padding-left:7em;"><a href=https://github.com/Manafredo/sapper-gl><i class="fab fa-github" style ="width: 2em; height: 2em"></i></a></li>
			</div>
		</div>
		<!--<li><a aria-current="{segment === 'about' ? 'page' : undefined}" href={"http://"+ $page.host + ""}>about</a></li>

		 for the blog link, we're using rel=prefetch so that Sapper prefetches
		     the blog data when we hover over the link or tap it on a touchscreen 
		<li><a rel=prefetch aria-current="{segment === 'blog' ? 'page' : undefined}" href="blog">blog</a></li> -->
	</ul>
	
</nav>

