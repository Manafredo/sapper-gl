<!-- This component will be loaded into every single page, this is very useful for stuff like navbars of footers -->

<!-- This Javascript piece  -->
<script lang="ts">

/*Lets try some material ui stuff*/
  
  let prominent = false;
  let dense = false;
  let secondaryColor = false;

	import { stores } from '@sapper/app';
	const {page} = stores();
	import Textfield, {Input, Textarea} from '@smui/textfield';
	let valueShapedOutlinedB = '';

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
						<Textfield class="shaped-outlined" variant="outlined" withLeadingIcon bind:value={valueShapedOutlinedB} label="Suche" input$aria-controls="helper-text-shaped-outlined-b" input$aria-describedby="helper-text-shaped-outlined-b" style="height: 50%">
						</Textfield>
					</div>
				</li>
			</div>
			<div class="flex-item">
				<li style="float: right; padding-left:7em;"><a href=https://github.com/Manafredo/sapper-gl><img src="github.png"  alt="Github"></a></li>
			</div>
		</div>
		<!--<li><a aria-current="{segment === 'about' ? 'page' : undefined}" href={"http://"+ $page.host + ""}>about</a></li>

		 for the blog link, we're using rel=prefetch so that Sapper prefetches
		     the blog data when we hover over the link or tap it on a touchscreen 
		<li><a rel=prefetch aria-current="{segment === 'blog' ? 'page' : undefined}" href="blog">blog</a></li> -->
	</ul>
</nav>




