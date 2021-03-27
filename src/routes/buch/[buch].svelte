<script>
    import {onMount} from "svelte";
    import { stores } from '@sapper/app';
    import Paper, {Title, Subtitle, Content} from '@smui/paper';

    let book =[];
    let persons = [];
    let personsarr =[];
    let commentsarr =[];
    let personsWithComments =[];
    let description;
    let author;
    let coverurl;
    let count = 1;
    let preview_description;

    const {page} = stores();
    let title = $page.params.buch;
    title = title.replace("_"," ");
    title = title.replace("%20"," ");
    title = title.replace("%20"," ");
    title = title.replace("%20"," ");
    title = title.replace("%20"," ");
    title = title.replace("%20"," ");
    title = title.replace("%20"," ");


    onMount(async function() {
      // fetching information from book
      const response = await fetch("https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?select=sys.id,fields&content_type=book&include=0&fields.title="+title+"&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
      );
      book = await response.json();
      book= book.items;
      description = book["0"]["fields"]["description"];
      //preview_description = description.substring(0,500)+ "...";
      author = book["0"]["fields"]["author"];
      coverurl = book["0"]["fields"]["coverurl"];
      // We need Amazon Link and 
      
      // fetching information from comments & persons
      const response3 = await fetch("https://cdn.contentful.com/spaces/t170cpyn3oju/environments/master/entries/?content_type=personBook&include=2&fields.book.sys.id="+book["0"]["sys"]["id"]+"&access_token=MFnR8m8akJLpWiIGbewXZi_PgdWJ0lWv46tjhf7g4uU"
      );
      let comments = await response3.json();
      persons = comments.includes.Entry;
      comments=comments.items;
      for (let i=0; i<comments.length; i++){
        let obj = comments[i];
        commentsarr.push({id: obj.fields.person.sys.id, sourcedescription: obj.fields.sourcedescription})
      }
      for (let i=0; i<persons.length; i++){
        let obj = persons[i];
        personsarr.push({id: obj.sys.id, name: obj.fields.name, description: obj.fields.description})
      }
      personsWithComments=commentsarr.map(t1 => ({...t1, ...personsarr.find(t2=>t2.id===t1.id)}));
    });
    
</script>

<style>
  
  .flex-container2 {
    display: flex;
    flex-flow: row wrap;
    justify-content: space-around;
  }
  .flex-item{
  padding: 1em 0.1em;
  width: 350px;  
}
  
  .flex-container {
  display: flex;}
  img {
    max-width: 200px;
  }
</style>

<main>
  <article style="margin: 2em 2em">
    <div style="display: flex; flex-wrap: wrap; align-items: center; justify-content: center;">
      <div class="paper-container">
        <Paper class="paper-person" style="background-color: #e6e6e6;">
        <div class="flex-container">
          <div style="display: flex; align-items: center; justify-content: center;">
              <img src={coverurl} alt={title} >
          </div>
          <div>
              <h1 style="color:#e6e6e6;">0</h1>
          </div>
          <div>
            <Title>{title}</Title>
            <Subtitle >{author}</Subtitle>
            <Content>{description}</Content>
          </div>
        </div>
        </Paper>
      </div>
    </div>
  </article>
  <article>
      <div class="flex-container2">
          {#each personsWithComments as person}
          <div class="flex-item">
          <div class="paper-container">
              <Paper class="paper-person" style="background-color: #e6e6e6;">
              <div class="flex-c">
              <Title>{person.name}</Title>
              <Content >{person.sourcedescription}</Content>
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
