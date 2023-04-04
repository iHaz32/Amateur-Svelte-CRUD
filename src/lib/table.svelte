<script lang="ts">
    import '../routes/style.css';
    import addIcon from '../static/addIcon.png';
    import editIcon from '../static/editIcon.png';
    import deleteIcon from '../static/deleteIcon.png';
    import checkIcon from '../static/checkIcon.png';
    import xIcon from '../static/xIcon.png';
    import { Menu, MenuButton, MenuItems, MenuItem } from '@rgossiaux/svelte-headlessui';
    let data = [
        {
            name: "Giorgos Kanelopoulos",
            ethnicity: "Hellenic",
            birth: 179046441,
            birthdate: "",
            id: 9824309,
            editing: false,
            deleting: false,
        },
        {
            name: "Giannis Mixailidis",
            ethnicity: "Hellenic",
            birth: 1227452346,
            birthdate: "",
            id: 7234982,
            editing: false,
            deleting: false,
        },
        {
            name: "Paris Ametoglou",
            ethnicity: "Turkish",
            birth: 1061780347,
            birthdate: "",
            id: 7297482,
            editing: false,
            deleting: false,
        },
        {
            name: "Giannis Omperoglou",
            ethnicity: "Turkish",
            birth: 771554963,
            birthdate: "",
            id: 8824233,
            editing: false,
            deleting: false,
        },
        {
            name: "Antonios Antoniou",
            ethnicity: "Hellenic",
            birth: 1097429851,
            birthdate: "",
            id: 2483202,
            editing: false,
            deleting: false,
        },
        {
            name: "Entri Toskou",
            ethnicity: "Albanian",
            birth: 456064569,
            birthdate: "",
            id: 4750732,
            editing: false,
            deleting: false,
        },
        {
            name: "Nikolaos Slidis",
            ethnicity: "Hellenic",
            birth: 871449126,
            birthdate: "",
            id: 4535323,
            editing: false,
            deleting: false,
        },
        {
            name: "Giannis Ioannidis",
            ethnicity: "Hellenic",
            birth: 388379578,
            birthdate: "",
            id: 2034832,
            editing: false,
            deleting: false,
        },
        {
            name: "Giorgos Papadopoulos",
            ethnicity: "Hellenic",
            birth: 1016329174,
            birthdate: "",
            id: 1840921,
            editing: false,
            deleting: false,
        },
        {
            name: "Petros Mixaj",
            ethnicity: "Albanian",
            birth: -60246568,
            birthdate: "",
            id: 2034092,
            editing: false,
            deleting: false,
        },
        {
            name: "Aristotelis-Zisis Papamixail",
            ethnicity: "Hellenic",
            birth: -255162253,
            birthdate: "",
            id: 3029581,
            editing: false,
            deleting: false,
        },
    ]
    let open=false;
    let displayingData=data;
    displayingData.map((el) => {
        let a = new Date(el.birth * 1000);
        let year = a.getFullYear();
        let month = a.getMonth();
        let date = a.getDate();
        el.birthdate=date+'/'+month+'/'+year;
    })
    let existingEthnicities= new Array();
    displayingData.map((el) => {existingEthnicities.unshift(el.ethnicity)});
    existingEthnicities = [...new Set(existingEthnicities)]; //Removes duplicates

    const add = (e: Event) => {
        e.preventDefault()
        const newObj = {
            name: "",
            ethnicity: "",
            birth: null,
            birthdate: "",
            id: data.length+1,
            editing: true,
            deleting: false,
        }
        data.unshift(newObj);
        displayingData = data; //yes ik that line is weird but i gotta exist
        if (newObj.ethnicity!=="") {
            existingEthnicities.unshift(newObj.ethnicity);
            existingEthnicities = [...new Set(existingEthnicities)]; //Removes duplicates
        }
    }
    const handleClick = (e: Event, el:any) => {
        e.preventDefault();
        if (el.editing) { //EDIT
            el.name=e.target[0].value;
            el.ethnicity=e.target[1].value;
            el.birthdate=e.target[2].value;
            el.id=e.target[3].value;
            el.birth=Math.floor(new Date(el.birthdate).getTime() / 1000);
            if (el.ethnicity!=="") {
            existingEthnicities.unshift(el.ethnicity);
            existingEthnicities = [...new Set(existingEthnicities)]; //Removes duplicates
            }
        } else { //DELETE
            if (displayingData.filter((citizen) => citizen.ethnicity === el.ethnicity).length === 1 ) { //Remove the ethnicity from filter list if it's the last one.
                //s
                existingEthnicities = existingEthnicities.filter((ethnicity) => ethnicity!==el.ethnicity);
            }
            data = displayingData.filter((citizen)=>citizen.id!==el.id);
            displayingData = data;
        }
    }
    const sort = (method: String) => {
        switch (method) {
            case "NA1":
                displayingData = displayingData.sort((c1, c2) => (c1.name > c2.name) ? 1 : (c1.name < c2.name) ? -1 : 0);
                break;
            case "NA2":
                displayingData = displayingData.sort((c1, c2) => (c1.name < c2.name) ? 1 : (c1.name > c2.name) ? -1 : 0);
                break;
            case "EA1":
                displayingData = displayingData.sort((c1, c2) => (c1.ethnicity > c2.ethnicity) ? 1 : (c1.ethnicity < c2.ethnicity) ? -1 : 0);
                break;
            case "EA2":
                displayingData = displayingData.sort((c1, c2) => (c1.ethnicity < c2.ethnicity) ? 1 : (c1.ethnicity > c2.ethnicity) ? -1 : 0);
                break;
            case "AA":
                displayingData = displayingData.sort((c1, c2) => (c1.birth > c2.birth) ? 1 : (c1.birth < c2.birth) ? -1 : 0);
                break;
            case "AD":
                displayingData = displayingData.sort((c1, c2) => (c1.birth < c2.birth) ? 1 : (c1.birth > c2.birth) ? -1 : 0);
                break;
            case "IA":
                displayingData = displayingData.sort((c1, c2) => (c1.id > c2.id) ? 1 : (c1.id < c2.id) ? -1 : 0);
                break;
            case "ID":
                displayingData = displayingData.sort((c1, c2) => (c1.id < c2.id) ? 1 : (c1.id > c2.id) ? -1 : 0);
                break;
        }
    }
    const filter = (input:string) => {
        displayingData = data.filter((el) => el.ethnicity===input);
    }
    const search = (input:string) => {
        if (input!=="")
            console.log(input);
            displayingData = data.filter((el) => el.name.toLowerCase().includes(input.toLowerCase()));
    }
</script>

<style>
    * {
        font-family: "Lucida Console", "Courier New", monospace;
    }

    table, th, td {
        border:1px solid black;
    }

    .button1 {
        align-items: center;
        appearance: none;
        background-color: #5f9ea0;
        border-radius: 4px;
        border-width: 0;
        box-sizing: border-box;
        cursor: pointer;
        justify-content: center;
        line-height: 1.2;
        padding-left: 8px;
        padding-right: 8px;
        transition: box-shadow .15s,transform .15s;
    }


    .button1:hover {
    transform: translateY(-2px);
    }

    .button1:active {
    transform: translateY(2px);
    }
</style>


<div class="flex flex-col justify-center items-center overflow-x-scroll sm:overflow-x-auto">
    <div class="flex flex-row justify-center border-2 border-black gap-2 sm:gap-6 md:gap-10 w-[250px] xs:w-[400px] sm:w-[500px] md:w-[600px]">
        <div class="flex justify-center gap-1 sm:gap-2 mt-1 mb-1 text-[10px] md:text-base">
            <Menu>
                <MenuButton>
                    <button class="button1 h-3 sm:h-4 md:h-5">Sort</button>
                </MenuButton>
                <MenuItems class="absolute flex flex-col text-[10px] sm:text-xs md:text-sm lg:text-base rounded-md shadow-lg bg-[#5f9ea0]">
                    <MenuItem class="border-2 border-black">
                        <button on:click={()=>sort("NA1")}>Naming Alphabetically (A-Z)</button>
                    </MenuItem>
                    <MenuItem class="border-2 border-black">
                        <button on:click={()=>sort("NA2")}>Naming Alphabetically (Z-A)</button>
                    </MenuItem>
                    <MenuItem class="border-2 border-black">
                        <button on:click={()=>sort("EA1")}>Ethnicity Alphabetically (A-Z)</button>
                    </MenuItem>
                    <MenuItem class="border-2 border-black">
                        <button on:click={()=>sort("EA2")}>Ethnicity Alphabetically (Z-A)</button>
                    </MenuItem>
                    <MenuItem class="border-2 border-black">
                        <button on:click={()=>sort("AA")}>Age (Ascending)</button>
                    </MenuItem>
                    <MenuItem class="border-2 border-black">
                        <button on:click={()=>sort("AD")}>Age (Descending)</button>
                    </MenuItem>
                    <MenuItem class="border-2 border-black">
                        <button on:click={()=>sort("IA")}>ID (Ascending)</button>
                    </MenuItem>
                    <MenuItem class="border-2 border-black">
                        <button on:click={()=>sort("ID")}>ID (Descending)</button>
                    </MenuItem>
                </MenuItems>
            </Menu>
            <Menu>
                <MenuButton>
                    <button class="button1 h-3 sm:h-4 md:h-5">Filter</button>
                </MenuButton>
                <MenuItems class="absolute flex flex-col text-[10px] sm:text-xs md:text-sm lg:text-base rounded-md shadow-lg bg-[#5f9ea0]">
                    <MenuItem class="border-2 border-black">
                        <Menu>
                            <MenuButton>
                                <button class="button1 h-3 sm:h-4 md:h-5">Ethnicity</button>
                            </MenuButton>
                            <MenuItems class="absolute flex flex-col text-[10px] sm:text-xs md:text-sm lg:text-base rounded-md shadow-lg bg-[#5f9ea0]">
                                {#each [...existingEthnicities] as ethnicity}
                                <MenuItem class="border-2 border-black">
                                    <button on:click={()=>filter(ethnicity)}>{ethnicity}</button>
                                </MenuItem>
                                {/each}
                            </MenuItems>
                        </Menu>
                    </MenuItem>
                </MenuItems>
            </Menu>
        </div>
        <form on:submit={(e)=>search(e.target[0].value)} class="flex justify-center gap-1 sm:gap-2 mt-1 mb-1 text-[10px] md:text-base">
            <input type="text" class="h-3 sm:h-4 md:h-5 w-20 sm:w-40 md:w-52">
            <button type="submit" class="button1 h-3 sm:h-4 md:h-5">Search</button>
        </form>
    </div>
    <div class="flex overflow-y-scroll w-[250px] xs:w-[400px] sm:w-[500px] md:w-[600px] h-[450px]">
        <table class="text-[10px] sm:text-sm md:text-base h-min w-full">
            <tr>
                <th>
                    <button class="mt-2" on:click={(e)=>add(e)}>
                        <img src={addIcon} alt="ADD" class="h-4 w-4 md:h-7 md:w-7" />
                    </button>
                </th>
                <th class="p-2">Name</th>
                <th class="p-2">Ethnicity</th>
                <th class="p-2">Birth</th>
                <th class="p-2">ID</th>
            </tr>
            {#each [...displayingData] as el}
                <tr>
                    <td>
                        <div class="flex flex-row">
                            {#if !el.editing&&!el.deleting}
                            <button on:click={()=>el.deleting=true} class="h-4 w-4 md:h-7 md:w-7">
                                <img src={deleteIcon} alt="D">
                            </button>
                            <button on:click={()=>el.editing=true} class="h-4 w-4 md:h-7 md:w-7">
                                <img src={editIcon} alt="E"/>
                            </button>
                            {:else}
                            <form method="GET" id="my_form" on:submit={(e)=>{handleClick(e,el);el.editing=false;el.deleting=false}}>
                                <input type="image" class="h-4 w-4 md:h-7 md:w-7" src={checkIcon} alt="Y">
                            </form>
                            <button type="button" class="h-4 w-4 md:h-7 md:w-7" on:click={()=>{el.editing?el.editing=false:el.deleting=false}}>
                                <img src={xIcon} alt="N"/>
                            </button>
                            {/if}
                        </div>
                    </td>
                    {#if !el.editing}
                        <td class="p-2">{el.name}</td>
                        <td class="p-2">{el.ethnicity}</td>
                        <td class="p-2">{el.birthdate}</td>
                        <td class="p-2">{el.id}</td>
                    {:else}
                        <td class="p-2">
                            <input type="text" value={el.name} class="text-center" form="my_form" />
                        </td>
                        <td class="p-2">
                            <input type="text" value={el.ethnicity} class="w-[100%] text-center" form="my_form" />
                        </td>
                        <td class="p-2">
                            <input type="text" value={el.birthdate} class="w-[100%] text-center" form="my_form" />
                        </td>
                        <td class="p-2">
                            <input type="text" value={el.id} class="w-[100%] text-center" form="my_form" />
                        </td>
                    {/if}
                </tr>
            {/each}
        </table>
    </div>
</div>