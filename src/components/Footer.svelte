<script lang="ts">

    /*Lets try some material ui stuff*/
        import { stores } from '@sapper/app';
        const {page} = stores();

    
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
            background-color: #FFFFFF;
            font-weight: 300;
            padding: 0 1em;
            display:flex;
            justify-content:space-around;
        }
    </style>
    
    <!-- This is the html part where we add the navbar -->
    <!-- Navbar is added using material ui components that are loaded in he script section - there is no linking yet-->
    <nav>
        <p>Made by Meltem, Elly and Andrej</p>
    </nav>
    
    
    
    
    