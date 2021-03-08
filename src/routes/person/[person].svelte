<script>
    import {onMount} from "svelte";
	import { stores } from '@sapper/app';
	import Paper, {Title, Content} from '@smui/paper';


    let books =[];
    let person = [];
    let bookarr =[];
    let commentarr=[];
    let booksWithComments=[];

    let name;
    let description;
	let pictureurl;
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
      
      const response2 = await fetch("https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?content_type=book&include=2&fields.person.sys.id="+person["0"]["sys"]["id"]+"&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
      );
      books = await response2.json();
      books=books.items;
      for (let i=0; i<books.length; i++){
        let obj = books[i];
        bookarr.push({id: obj.sys.id, title: obj.fields.title, description: obj.fields.description})
      }
      //console.log(bookarr);

      const response3 = await fetch("https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?content_type=personBook&include=2&fields.person.sys.id="+person["0"]["sys"]["id"]+"&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
      );
      let comments = await response3.json();
      comments=comments.items;
      for (let i=0; i<comments.length; i++){
        let obj = comments[i];
        commentarr.push({id: obj.fields.book.sys.id, sourcedescription: obj.fields.sourcedescription})
      }
      //console.log(commentarr);
      booksWithComments=bookarr.map(t1 => ({...t1, ...commentarr.find(t2=>t2.id===t1.id)}));
      //console.log(booksWithComments);

    });
    
</script>
<link rel="stylesheet" href="https://use.typekit.net/cal1lzu.css">
<style>
.flex-container {
  display: flex;}
img {
  border-radius: 50%;
  max-width: 200px;
}

</style>
<main>
  <div style="display: flex; flex-wrap: wrap;">
	<div>
		<div class="paper-container">
		  <Paper class="paper-person" style="background-color: #e6e6e6;">
			<div class="flex-container">
			  <div style="display: flex; align-items: center; justify-content: center;">
				<img src={pictureurl} alt={name} >
			  </div>
			  <div>
				<h1 style="color:#e6e6e6;">0</h1>
			</div>
			  <div>
			<Title>{name}</Title>
			<Content>{description}</Content>
			</div>
		  </Paper>
		</div>
	  </div>
  </div>
    <ul>
      {#each booksWithComments as book}
          <a href={"http://localhost:3000/buch/" + book.title}>{book.title}</a>
          <p>{book.description}</p>
          <p>Source: {book.sourcedescription}</p>
      {:else}
      <!-- this block renders when book.length === 0 -->
      <p>loading...</p>
      {/each}
    </ul>
  </main>
