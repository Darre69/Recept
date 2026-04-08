<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap" rel="stylesheet">
<script>
    let showPopUp = $state(false);

    let rubrik = $state("");
    let bild = $state("");
    let tid = $state("");
    let lank = $state("");

    // 💾 LOAD från localStorage
    let data = $state([]);

    if (typeof localStorage !== "undefined") {
        const saved = localStorage.getItem("cards");
        if (saved) {
            data = JSON.parse(saved);
        }
    }

    function openPopUp() {
        showPopUp = true;
    }

    function closePopUp() {
        showPopUp = false;
    }

    function saveItem() {
        if (!rubrik) return;

        data = [
            ...data,
            {
                id: Date.now(),
                rubrik,
                bild,
                tid,
                lank
            }
        ];

        rubrik = "";
        bild = "";
        tid = "";
        lank = "";

        showPopUp = false;
    }

    // 💾 AUTO SAVE varje gång data ändras
    $effect(() => {
        localStorage.setItem("cards", JSON.stringify(data));

    });
    
    function deleteItem(id) {
    data = data.filter(item => item.id !== id);
}
</script>

<div class="hela">

    <div class="header">
        <button class="add" onclick={openPopUp}>Lägg till</button>
    </div>

    <div class="main">
        <h1 class="storText">Favorit Recept</h1>
    </div>

    <!-- POPUP -->
    {#if showPopUp}
    <div class="popUp">
        <div class="popUpInputs">

            <input placeholder="Rubrik" bind:value={rubrik} />
            <input placeholder="Bild URL" bind:value={bild} />
            <input placeholder="Tid" bind:value={tid} />
            <input placeholder="Länk" bind:value={lank} />

            <button onclick={saveItem} class="popUpKnappar">Spara</button>
            <button onclick={closePopUp} class="popUpKnappar">Stäng</button>

        </div>
    </div>
    {/if}

    <!-- CARDS -->
    <div class="cards">

        {#each data as item (item.id)}
        <div class="card" onclick={() => item.lank && window.open(item.lank, "_blank")}>

            {#if item.bild}
                <img src={item.bild} alt="bild" class="card-img" />
            {/if}

            <div class="card-content">
                <h3>{item.rubrik}</h3>
                <p>{item.tid}</p>
                <button
  class="deleteKnapp"
  onclick="{(e) => { e.stopPropagation(); deleteItem(item.id); }}"
>
  Ta bort
</button>
               

            </div>

        </div>
        {/each}

    </div>

</div>

<style>
.hela {
    font-family: 'Poppins';
    
}

.header {
    width: 100%;
    height: 50px;
    background: rgb(242, 242, 242);
    display: flex;
    align-items: center;
    border-radius: 4px;
}

.add {
    margin-left: auto;
    font-weight: 600;
    cursor: pointer;
    margin-right: 35px;
    border: 2px solid white;
    color: white;
    background-color: rgb(167, 163, 163);
    border-radius: 6px;
    padding: 5px;
    padding-left: 14px;
    padding-right: 14px;

}

.add:hover {
    transform: scale(1.05);
}

.add:active {
    transform: scale(0.95);
}

.main {
    height: 550px;
    background-image: url("bild.JPG");
    background-size: cover;
    background-position: center;
}

.storText {
    padding-top: 140px;
    font-size: 150px;
    display: flex;
    justify-content: center;
    align-items: center;
    color: white;
    font-weight: 700;
    cursor: default;
      -webkit-text-stroke: 1px black;

}

/* POPUP */
.popUp {
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.5);
    display: flex;
    justify-content: center;
    align-items: center;
}

.popUpInputs {
    background: white;
    padding: 40px;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
    gap: 15px;
    width: 400px;
    height: 300px;
}

input {
    border: 2px solid rgba(186, 180, 180, 0.538);
    border-radius: 4px;
    padding-left: 4px;
}

.popUpKnappar {
    background-color: rgb(223, 217, 217);
    border: 2px solid rgba(186, 180, 180, 0.538);
    border-radius: 4px;
    cursor: pointer;
}

.popUpKnappar:hover {
    transform: scale(1.02);
}

.popUpKnappar:active {
    transform: scale(0.95);
}

/* CARDS */
.cards {
    padding: 20px;
    display: grid;
    gap: 20px;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
}

.card {
    background: white;
    border-radius: 10px;
    overflow: hidden;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    cursor: pointer;
}
.card:hover {
    transform: scale(1.02 );
}

.card-img {
    width: 100%;
    height: 300px;
    object-fit: cover;
}
h3 {
    font-size: 26px;
    font-weight: 500;

}
.deleteKnapp {
    font-size: 13px;
    cursor: pointer;

}
.deleteKnapp:hover {
    text-decoration: underline;
}
.card-content {
    padding: 15px;

}

</style>