<script>
  	//onMount starts stuff before loading the page
    import {onMount} from "svelte";
    //INfo about Parameters
	  import { stores } from '@sapper/app';
    import "@fortawesome/fontawesome-free/js/all.min.js"
    // Material UI for Svelte
	  import Paper, {Title,Subtitle, Content} from '@smui/paper';
    import Button, {Icon, Label} from '@smui/button'
    import '../button.scss';

    // Variables for fetched data from contentful
    let books =[];
    let person = [];
    let bookarr =[];
    let commentarr=[];
    let booksWithComments=[];

    // Variables for selected person
    let name;
    let description;
	  let pictureurl;

    // Getting personanme from parameters
	  const {page} = stores();
    let personName = $page.params.person;
    personName = personName.replace("_"," ");

    onMount(async function() {
      const response = await fetch("https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?select=sys.id,fields&content_type=person&include=0&fields.name="+personName+"&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
      );
      person = await response.json();
      person= person.items;
      name=person["0"]["fields"]["name"];
      description = person["0"]["fields"]["description"];
	    pictureurl=person["0"]["fields"]["pictureurl"];
      //console.log(person);
      
      //Get all bokks from selected person
      const response2 = await fetch("https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?content_type=book&include=2&fields.person.sys.id="+person["0"]["sys"]["id"]+"&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
      );
      books = await response2.json();
      books=books.items;
      for (let i=0; i<books.length; i++){
        let obj = books[i];
        bookarr.push({id: obj.sys.id, title: obj.fields.title, description: obj.fields.description, cover: obj.fields.coverurl, amazonlink: obj.fields.amazonurl, author: obj.fields.author})
      }
      //console.log(bookarr);

      //Get all comments from selected person
      const response3 = await fetch("https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?content_type=personBook&include=2&fields.person.sys.id="+person["0"]["sys"]["id"]+"&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
      );
      let comments = await response3.json();
      comments=comments.items;
      for (let i=0; i<comments.length; i++){
        let obj = comments[i];
        commentarr.push({id: obj.fields.book.sys.id, sourcedescription: obj.fields.sourcedescription, sourceurl: obj.fields.sourceurl})
      }
      //console.log(commentarr);
      //Mapping Informnation from Books and Comments
      booksWithComments=bookarr.map(t1 => ({...t1, ...commentarr.find(t2=>t2.id===t1.id)}));
      //console.log(booksWithComments);
    });

    function handleClick(URL) {
      window.open(URL, '_blank')
	}
    
</script>

<!-- Necessary for styling of person - making sure pictures to the left of the text-->
<style>
  * :global(.myClass) {
    text-decoration: underline !important;
  }
  * :global(.mdc-button, .smui-button__group) {
    margin-bottom: .4em;
  }
  * :global(.smui-button__group .mdc-button) {
    margin-bottom: 0;
  }
  
.flex-container {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-around;
}
.flex-item{
  padding: 1em 0.1em;
  width: 350px;  
}
a{
  text-decoration: none;
}

</style>

<!-- paper style from material ui-->
<main>
  <article style="margin: 2em 2em">
<!-- display of person-->
  <div style="display: flex; flex-wrap: wrap; align-items: center; justify-content: center;">
	<div>
		<div class="paper-container">
		  <Paper class="paper-person" style="background-color: #e6e6e6;">
			  <div class="flex-container">
			    <div style="display: flex; align-items: center; justify-content: center;">
                <img src={pictureurl} alt={name} style="border-radius: 50%; max-width: 200px; padding-right: 1em;">
			    </div>
			    <div>
			      <Title>{name}</Title>
			      <Content>{description}</Content>
			  </div>
		  </Paper>
		</div>
	  </div>
  </div>
</article>
  <!-- show books with comments-->
  <article style="margin: 2em 0">
    <div class="flex-container" >
      {#each booksWithComments as book}
        <div class=flex-item>
          <div class="paper-container">
            <Paper class="paper-person" style="background-color: #e6e6e6;">
              <div class="flex-container">
                <div style="display: flex; align-items: center; justify-content: center;">
                  <a href={"./buch/" + book.title}>
                    <img src={book.cover} alt={name}  style=" max-width: 120px; padding-right: 1em;" >
                  </a>
                </div>
                <div>
                  <Title><a href={"./buch/" + book.title}>{book.title}</a></Title>
                  <Subtitle>{book.author}</Subtitle>
                  <Content>{book.description.substring(0,100)+ " ..."}</Content>
                  <a  href={book.sourceurl} target="_blank" rel="noopener noreferrer">Bewertung: {book.sourcedescription}</a>
                  <div style="padding-top: 1em">
                    <Button variant="outlined" on:click={handleClick(book.amazonlink)}><i class="fab fa-amazon" style="color: #ff922b; margin-right: 1em"></i><Label>  Auf Amazon ansehen</Label></Button>                
                  </div>
                  <!--
                  <div style="padding-top: 1em">
                    <Button variant="outlined"><img src="thalia.svg" alt="Thalia" style="width: 40px"><Label>  Auf Amazon ansehen</Label></Button>                
                  </div>
                -->
              </div>
            </Paper>
          </div>
        </div>
      {:else}
      <!-- this block renders when book.length === 0 -->
      <p>loading...</p>
      {/each}
      </div>
  </article>   
  </main>
