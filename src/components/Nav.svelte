<!-- This component will be loaded into every single page, this is very useful for stuff like navbars of footers -->

<!-- This Javascript piece  -->
<script lang="ts">

/*Lets try some material ui stuff*/
  
	export let segment;
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
	ul {
		margin: 0;
		padding: 0;
	}
	/* clearfix */
	ul::after {
		content: '';
		display: block;
		clear: both;
	}
	li {
		display: block;
		float: left;
	}
	[aria-current] {
		position: relative;
		display: inline-block;
	}
	[aria-current]::after {
		position: absolute;
		content: '';
		width: calc(100% - 1em);
		height: 2px;
		background-color: #FF9700;
		display: block;
		bottom: -1px;
	}
	a {
		text-decoration: none;
		padding: 1em 0.5em;
		display: block;
	}
	img {
		text-decoration: none;
		padding: 0.5em 0;
		display: block;
	}
</style>

<!-- This is the html part where we add the navbar -->
<!-- Navbar is added using material ui components that are loaded in he script section - there is no linking yet-->
<nav>
	<ul>

		<li>
			<img src="logo_lang.png" alt="Logo" style="height:40px;">
		</li>
		<li><a aria-current="{segment === undefined ? 'page' : undefined}" href={"http://"+ $page.host + ""}>home</a></li>
		<!--<li><a aria-current="{segment === 'about' ? 'page' : undefined}" href={"http://"+ $page.host + ""}>about</a></li>

		 for the blog link, we're using rel=prefetch so that Sapper prefetches
		     the blog data when we hover over the link or tap it on a touchscreen 
		<li><a rel=prefetch aria-current="{segment === 'blog' ? 'page' : undefined}" href="blog">blog</a></li> -->
	</ul>
</nav>




