<script>


    export let timer = 1000 + Math.floor(Math.random()*1000)
    let RNG
    let randomFish
    let display = ""

    let eligableFish = ["44$ blobfish", " 66$ lavahorse"]
    let rarity = ["1. BRONZE", "2. SILVER", "3. GOLD", "4. DIAMOND"]
    let items = []
    let fishCaught = []


    export let trophyChance = 50
    export let money = 0
    let test = ""
    export let fishingSpeed = 0
    
    export let charmTier = 0
    export let midasTier = 1
    export let radiantTier = 0




    function FishingRNG(){
         RNG = Math.floor(Math.random()*100) 
        randomFish = Math.floor(Math.random()*eligableFish.length)

            if (RNG <= trophyChance) {
                let i = Math.floor(Math.random()*1000)
                if (i <=2 * (2 * radiantTier)/100 + 1) {
                    fishCaught = [...fishCaught, {type: eligableFish[randomFish], rarity: rarity[3]}]
                     display = "You caught a " + eligableFish[randomFish] + " trophy fish! " + rarity[3]
                     fishingStatus = ""
                     
                }
                else if (i <=22 * ((2 * midasTier)/100 + 1)) {
                    fishCaught = [...fishCaught, {type: eligableFish[randomFish], rarity: rarity[2]}]
                     display = "You caught a " + eligableFish[randomFish] + " trophy fish! " + rarity[2]
                     fishingStatus = ""
                }
                else if (i <=275) {
                    fishCaught = [...fishCaught, {type: eligableFish[randomFish], rarity: rarity[1]}]
                     display = "You caught a " + eligableFish[randomFish] + " trophy fish! " + rarity[1]
                     fishingStatus = ""
                }
                else {
                    fishCaught = [...fishCaught, {type: eligableFish[randomFish], rarity: rarity[0]}]
                     display = "You caught a " + eligableFish[randomFish] + " trophy fish! " + rarity[0]
                     fishingStatus = ""
                     

                     
                }
            }
            else {
                display = "You didn't get a fish."
                fishingStatus = ""
            }
    }

    function useTime () {
        display = "fishing..."
        setTimeout(FishingRNG, timer)
    }


function coinCalc (sellableFish, fishRarity) {
    sellableFish = parseInt(sellableFish)
    fishRarity = parseInt(fishRarity)
    return sellableFish * fishRarity 
}


function sellAllFish () {
    for (let i = 0; i < fishCaught.length; i++) {
       money += coinCalc(fishCaught[i].type, fishCaught[i].rarity)
    }
    fishCaught = []

}


 function purchase(price, item, Tchance, Fspeed) {

    if (money >= price) {
        money -= price;
        items = [...items, {name: item, trophyChance: Tchance, fishingSpeed: Fspeed }]
        timer -= Fspeed * 14.2857143
        trophyChance = trophyChance * ((Tchance/100)+1)
        fishingSpeed += Fspeed
}
}


</script>


<h1>Trophy fishing</h1>




<h3>
    {display} <br>
    {test}

</h3>



<button on:click={useTime}>fish up</button>

<button on:click={sellAllFish}>sell fish</button>


{#each fishCaught as fish}
<p>{fish.type} {fish.rarity}</p>
{/each}

<div id="cash">
<p>money: {money}$</p>
<p>☂ Fishing Speed: {fishingSpeed}</p>
<p>🏆Trophy chance: {Math.round(trophyChance)}%</p>
</div>

<br>


<div class="grid-container">
    <div class="grid-item" id="dropdown">Magma Rod $50<br>
        <button on:click={
        function purchaseItem () {
            if (items.some(e => e.name === "Magma Rod")) {} else {purchase(50, "Magma Rod", 10, 40) }
        }
        }>purchase</button>
    </div>
    <div class="grid-item">Inferno Rod $100 <br>
        <button on:click={
            function upgrade () {
                if (items.some(e => e.name === "Inferno Rod")) {}
                else if (items.some(e => e.name === "Magma Rod")) { purchase(100, "Inferno Rod", 5, 20) }
            }
            }>purchase</button>

    </div>
    <div class="grid-item">Hellfire <br> rod <br>
        <button on:click={
            function upgrade () {
                if (items.some(e => e.name === "Hellfire Rod")) {}
                else if (items.some(e => e.name === "Magma Rod") & items.some(e => e.name === "Inferno Rod")) 
                { purchase(1000, "Hellfire Rod", 5, 15) }
            }
            }>purchase</button>

    </div>

    <div class="grid-item">silver armor $100 <br>
        <button on:click={
            function purchaseItem () {
                if (items.some(e => e.name === "silver armor")) {} else {purchase(100, "silver armor", 5, 0) }
            }
            }>purchase</button>
    </div>
    <div class="grid-item">gold armor $1,000
        <button on:click={
            function upgrade () {
                if (items.some(e => e.name === "gold armor")) {}
                else if (items.some(e => e.name === "silver armor")) { purchase(1000, "gold armor", 5, 0) }
            }
            }>purchase</button>
    </div>
    <div class="grid-item">diamond armor $10,000 <br>
        <button on:click={
            function upgrade () {
                if (items.some(e => e.name === "diamond armor")) {}
                else if (items.some(e => e.name === "gold armor")) { purchase(10000, "diamond armor", 5, 0) }
            }
            }>purchase</button>
    </div>
    <div class="grid-item">Charm ${8 ** (charmTier+1) }
        <button on:click={
            function TierPurchase() {
                if (charmTier < 5 & money>= (8** (charmTier + 1))) {
                    money -= 8 ** (charmTier + 1)
                    charmTier += 1
                    trophyChance = trophyChance * 1.02 }
 }
            }>purchase</button>
    </div>
    <div class="grid-item">Midas Lure
        <button on:click={
            function TierPurchase() {
                if (midasTier < 10 & money>= (8** (midasTier + 1))) {
                    money -= 4 ** (midasTier + 1)
                    midasTier += 1 }
 }
            }>purchase</button>
    </div>
    <div class="grid-item">Radiant Fisher
        <button on:click={
            function TierPurchase() {
                if (radiantTier < 10 & money>= (8** (radiantTier + 1))) {
                    money -= 4 ** (radiantTier + 1)
                    radiantTier += 1 }
 }
            }>purchase</button>
    </div>
</div>


<style>
    #cash {
        position: absolute;
        left: 70%;
        top: 0%;
        color: var(--primary);
        font-size: 2opx;
    }
    .grid-container {
        display: grid;
        grid-template-columns: auto auto auto;
        position: absolute;
        left: 58%;
        top: 20%;
        gap: 10px;
        background-color: var(--background);
        padding: 10px;
    }
.grid-item {
  background-color: var(--secondary);
  border: 1px solid rgba(0, 0, 0, 0.8);
  padding: 20px;
  font-size: 30px;
  text-align: center;
}
button {
    border: 2px solid black;
  background-color: var(--secondary);
  color: var(--text);
  padding: 8px 12px;
  cursor: pointer;
  border-radius: 5px;
}
button:hover {
    background-color: var(--accent);
}
button:active { background-color: var(--secondary);
}

:root {
    --text: #f3e4e3;
			--background: #0c0505;
			--primary: #dc9c9a;
			--secondary: #7d2926;
			--accent: #ca403c;
}

</style>

