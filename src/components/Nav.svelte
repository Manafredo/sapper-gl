<!-- This component will be loaded into every single page, this is very useful for stuff like navbars of footers -->

<!-- This Javascript piece  -->
<script lang="ts">

/*Lets try some material ui stuff*/
  

  let prominent = false;
  let dense = false;
  let secondaryColor = false;

	import { stores } from '@sapper/app';
	const {page} = stores();
	import Tab, {Icon, Label} from '@smui/tab';
	import TabBar from '@smui/tab-bar';
	let keyedTabs = [
		{
		k: 1,
		label: 'Home',
		path: '/'
		},
		{
		k: 2,
		label: 'Buch',
		path: '/buch'
		},
		{
		k: 3,
		label: 'Impressum',
		path: '/impressum',
		}
	];
	let keyedTabsActive = keyedTabs[0];



/* This is code that might be used for implementing the search in the navbar
  import Textfield, {Input, Textarea} from '@smui/textfield';
  import Icon from '@smui/textfield/icon/index';
  import HelperText from '@smui/textfield/helper-text/index';
  let valueFullwidth = '';
*/
  
</script>

<!-- This is the css part with  sapper / svelte standard styling we took from the example  -->
<!-- Here we should try to implement the colors from Meltems Design Colourcoding -->
<style>
	nav {
		background-color: #EFEFEF;
		border-bottom: 1px solid #B1B1B1;
		font-weight: 300;
		padding: 0 1em;
	}
	
</style>

<!-- This is the html part where we add the navbar -->
<!-- Navbar is added using material ui components that are loaded in he script section - there is no linking yet-->

<nav>
	<img src="static\logo-192.png" alt="logo">
	<TabBar tabs={keyedTabs} let:tab>
		<Tab {tab} href={"http://"+ $page.host +tab.path} >
		  <Label>{tab.label}</Label>
		</Tab>
	  </TabBar>
</nav>




