<script lang="ts">
</script>
    
    <!-- This is the css part with  sapper / svelte standard styling we took from the example  -->
    <!-- Here we should try to implement the colors from Meltems Design Colourcoding -->
    <style>
        
        nav {
            font-weight: 300;
            padding: 1em 1em;
            background-color: #EFEFEF;
		    border-top: 1px solid #B1B1B1;
		    }
            .flex-box {
		    display:flex;
            justify-content:space-between;
            }
	        li {
		    display: block;
	        }
           
    </style>
    
    <!-- This is the html part where we add the navbar -->
    <!-- Navbar is added using material ui components that are loaded in he script section - there is no linking yet-->
    <nav>
        <ul>
            <div class="flex-box">
                <!-- invisible flex item to make sure we have middle and right covered-->
                <div class="flex-item"></div>
                <div class="flex-item">
                    <li>Made with 💛 by Meltem, Elly and Andrej</li>
                </div>
                <div class="flex-item">
                    <li>      
                        <a href={"/impressum"}>Impressum</a>
                    </li>
                </div>
                
            </div>
        </ul>
    </nav>

    
    
    
    
    