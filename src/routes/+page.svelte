<script>
    let albums = [
        {
            imageUrl:
                "https://shopoliviarodrigo.fr/cdn/shop/files/limitededitionpurplevinyl-storeexclusive.png?v=1687796772&width=3000",
            title: "Guts",
            artist: "Olivia Rodrigo",
            price: 29.99,
        },
        {
            imageUrl:
                "https://cdn.dutycast.com/thesoundofvinyl.com/cdn/shop/products/1001363ae90b-a36b0d2722e6-image002_3.png",
            title: "emails i can't send",
            artist: "Sabrina Carpenter",
            price: 42.55,
        },
        {
            imageUrl:
                "https://interscope.com/cdn/shop/products/B2D---Vinyl_0003_NFR---Vinyl.png?v=1701117161",
            title: "Norman F***ing Rockwell",
            artist: "Lana Del Rey",
            price: 28.89,
        },
        {
            imageUrl:
                "https://shopfr.nickiminajofficial.com/cdn/shop/files/LPSTANDARD.png?v=16970382936",
            title: "Pink Friday 2",
            artist: "Nicki Minaj",
            price: 32.99,
        },
        {
            imageUrl:
                "https://shopca.billieeilish.com/cdn/shop/files/EILISHBILLDONTSMILEA_2019-11-06_13-19-30_C2LQK2373a_2048x_e2107434-a107-4fbc-bf26-1df8ee27ef63_1_1024x1024.png?v=1682449221",
            title: "Don't Smile At Me",
            artist: "Billie Eilish",
            price: 24.99,
        },
    ];

    let ajoutees = [];
    let showTooltip = false;

    function ajouterAuPanier(album) {
        const index = ajoutees.findIndex((a) => a.title === album.title);
        if (index !== -1) {
            ajoutees[index].count++;
        } else {
            ajoutees = [...ajoutees, { ...album, count: 1 }];
        }
    }

    function retirerDuPanier(album) {
        const index = ajoutees.findIndex((a) => a.title === album.title);
        if (index !== -1) {
            if (ajoutees[index].count > 1) {
                ajoutees[index].count--;
            } else {
                ajoutees = ajoutees.filter((_, i) => i !== index);
            }
        }
    }

    function retirerAlbum(album) {
        ajoutees = ajoutees.filter((a) => a.title !== album.title);
    }

    function viderPanier() {
        ajoutees = [];
    }

    function acheter() {
        alert("Achat effectué !");
        viderPanier(); // Vider le panier après achat
    }
</script>

<header class="header">
    <div class="header-left">
        <img
            class="logo"
            src="https://cdn.pixabay.com/photo/2017/04/19/10/24/vinyl-2241789_960_720.png"
            alt="Logo Vinyl"
        />
        <h1>Merch Vinyles</h1>
    </div>
    <div class="header-right">
        <button class="empty-cart-btn" on:click={viderPanier}
            >Vider le panier</button
        >
        <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
        <div class="panier-count">
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
            <img
                class="panier"
                src="https://www.pngplay.com/wp-content/uploads/3/Panier-PNG-De-Fichier-Telecharger.png"
                alt="Panier"
                on:click={() => (showTooltip = !showTooltip)}
            />
            <span class="count"
                >{ajoutees.reduce(
                    (total, album) => total + album.count,
                    0,
                )}</span
            >
            {#if showTooltip}
                <!-- svelte-ignore a11y-no-static-element-interactions -->
                <div
                    class="cart-tooltip"
                    on:mouseenter={() => (showTooltip = true)}
                    on:mouseleave={() => (showTooltip = false)}
                >
                    {#if ajoutees.length === 0}
                        <p>Panier vide</p>
                    {:else}
                        <ul>
                            {#each ajoutees as album}
                                <li>
                                    <img
                                        src={album.imageUrl}
                                        alt={album.title}
                                        class="tooltip-image"
                                    />
                                    <div class="tooltip-info">
                                        <span class="album-title"
                                            >{album.title}</span
                                        >
                                        <span class="album-artist"
                                            >{album.artist}</span
                                        >
                                        <span class="album-count"
                                            >Quantité:
                                            <button
                                                class="quantity-btn"
                                                on:click={() =>
                                                    retirerDuPanier(album)}
                                                >-</button
                                            >
                                            {album.count}
                                            <button
                                                class="quantity-btn"
                                                on:click={() =>
                                                    ajouterAuPanier(album)}
                                                >+</button
                                            >
                                        </span>
                                        <span class="album-price"
                                            >Prix: {(
                                                album.price * album.count
                                            ).toFixed(2)}€</span
                                        >
                                        <button
                                            class="remove-btn"
                                            on:click={() => retirerAlbum(album)}
                                            >Retirer</button
                                        >
                                    </div>
                                </li>
                            {/each}
                        </ul>
                        <div class="cart-summary">
                            <p>
                                Total: {ajoutees
                                    .reduce(
                                        (total, album) =>
                                            total + album.price * album.count,
                                        0,
                                    )
                                    .toFixed(2)}€
                            </p>
                            <button class="buy-btn" on:click={acheter}
                                >Acheter</button
                            >
                        </div>
                    {/if}
                </div>
            {/if}
        </div>
    </div>
</header>

<div class="background">
    {#each albums as album}
        <div class="list">
            <!-- svelte-ignore a11y-img-redundant-alt -->
            <img src={album.imageUrl} alt="Image de l'album" loading="lazy" />
            <p>{album.title}</p>
            <small>{album.artist}</small>
            <p>{album.price.toFixed(2)}€</p>
            <button on:click={() => ajouterAuPanier(album)}
                >Ajouter au panier</button
            >
            <button on:click={() => retirerDuPanier(album)}
                >Retirer du panier</button
            >
        </div>
    {/each}
</div>

<style>
    :global(body) {
        background: #f7f7f7;
        color: #333;
        font-family: "Helvetica Neue", Arial, sans-serif;
        margin: 0;
        padding: 0;
    }

    .header {
        background-color: #2c3e50;
        color: #fff;
        padding: 10px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding-left: 40px;
        padding-right: 40px;
    }

    .header-left {
        display: flex;
        align-items: center;
    }

    .header-right {
        display: flex;
        align-items: center;
        position: relative;
    }

    .logo {
        height: 60px;
        margin-right: 10px;
    }

    .panier {
        height: 30px;
        width: auto;
        vertical-align: middle;
        cursor: pointer;
    }

    .panier-count {
        position: relative;
        display: inline-block;
        margin-left: 10px;
    }

    .panier-count .count {
        position: absolute;
        top: -8px;
        right: -8px;
        background-color: #e74c3c;
        color: #fff;
        border-radius: 50%;
        padding: 4px;
        font-size: 12px;
    }

    .cart-tooltip {
        position: absolute;
        top: 40px;
        right: 0;
        background-color: #fff;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
        padding: 15px;
        border-radius: 5px;
        z-index: 1000;
        width: 300px;
    }

    .cart-tooltip ul {
        list-style-type: none;
        padding: 0;
        margin: 0;
    }

    .cart-tooltip ul li {
        display: flex;
        margin-bottom: 10px;
        align-items: center;
    }

    .cart-tooltip ul li img.tooltip-image {
        width: 50px;
        height: 50px;
        margin-right: 10px;
        border-radius: 5px;
    }

    .tooltip-info {
        display: flex;
        flex-direction: column;
    }

    .album-title {
        font-weight: bold;
    }

    .album-artist {
        font-size: 14px;
        color: #888;
    }

    .album-count {
        font-size: 14px;
        color: #444;
        display: flex;
        align-items: center;
    }

    .quantity-btn {
        background-color: #3498db;
        color: #fff;
        border: none;
        padding: 5px 10px;
        margin: 0 5px;
        border-radius: 3px;
        cursor: pointer;
        transition: background-color 0.3s ease;
    }

    .quantity-btn:hover {
        background-color: #2980b9;
    }

    .album-price {
        font-size: 14px;
        color: #444;
    }

    .remove-btn {
        background-color: #e74c3c;
        color: #fff;
        border: none;
        padding: 5px 10px;
        border-radius: 3px;
        cursor: pointer;
        margin-top: 5px;
        transition: background-color 0.3s ease;
    }

    .remove-btn:hover {
        background-color: #c0392b;
    }

    .cart-summary {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-top: 10px;
        border-top: 1px solid #ddd;
        padding-top: 10px;
    }

    .cart-summary p {
        color: #3498db;
        font-weight: bold;
    }

    .buy-btn {
        background-color: #3498db;
        color: #fff;
        border: none;
        padding: 10px 20px;
        border-radius: 5px;
        cursor: pointer;
        transition: background-color 0.3s ease;
    }

    .buy-btn:hover {
        background-color: #2980b9;
    }

    .background {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        padding: 20px;
    }

    .list {
        width: 220px;
        height: 360px;
        background-color: #fff;
        border-radius: 10px;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        margin: 15px;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        align-items: center;
        padding: 15px;
        transition: transform 0.3s ease;
    }

    .list:hover {
        transform: translateY(-5px);
    }

    .list img {
        width: 100%;
        height: 200px;
        object-fit: cover;
        border-top-left-radius: 10px;
        border-top-right-radius: 10px;
        margin-bottom: 10px;
    }

    .list p {
        font-size: 18px;
        font-weight: bold;
        margin: 0;
    }

    .list small {
        font-size: 14px;
        color: #888;
        margin-bottom: 5px;
    }

    .list button {
        width: 80%;
        margin-top: auto;
        background-color: #3498db;
        color: #fff;
        border: none;
        padding: 10px 20px;
        border-radius: 5px;
        cursor: pointer;
        transition: background-color 0.3s ease;
    }

    .list button:hover {
        background-color: #2980b9;
    }

    .empty-cart-btn {
        background-color: #e74c3c;
        color: #fff;
        border: none;
        padding: 10px 20px;
        border-radius: 5px;
        cursor: pointer;
        transition: background-color 0.3s ease;
        margin-left: 10px;
    }

    .empty-cart-btn:hover {
        background-color: #c0392b;
    }

    @media (max-width: 768px) {
        .list {
            width: calc(50% - 30px);
        }
    }
</style>
