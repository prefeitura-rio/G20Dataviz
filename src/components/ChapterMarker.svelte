<script>
    import { onMount } from "svelte";
    import { activeSection, readingList, readingListVisible, sliderVisible } from "$stores/misc.js";
    import { fly, fade } from 'svelte/transition';
    import Icon from "$components/helpers/Icon.svelte";
    import {select} from "d3-selection";
    import Range from "$components/helpers/Range.svelte";
    import logo from "$svg/logo.svg";
    import logo_g20 from "$img/pointer.png";

    const sections = ["intro", "raunchiness", "illustration", "race", "methods"];
	let sliderValRaunch;
    let sliderValIllo;
    let sliderValRace;
    let rangeStart = 100;
    let loading = true;

    onMount(() => {
		loading = false;
	})

    function hideSlide(activeSection) { sliderVisible.set(false); }

    $: $activeSection, hideSlide($activeSection)
</script>

<nav>
    <div class="top-nav" class:loading={loading}>
        <button class="listBtn" aria-label="open reading list">
            <a href="https://www.g20.rio/">
                <img src="assets/images/g20_logo_pt.png">
            </a>
         </button>

        <!-- <div class="sect-btns">
            {#each sections as section, i}
                {@const active = $activeSection == section ? "active" : ""}
                <div class="btn-wrapper"
                    class:isHidden={$readingListVisible}>
                    <button 
                        on:click={handleChapterClick}
                        class="sectionBox {active}"
                        id="sectionBox-{section}"
                        aria-label="jump to {section}">
                    </button>
                    <p class="label {active}">{resetTitles(section)}</p>
                </div>
            {/each}
        </div> -->
        <div class="logo" class:isHidden={$readingListVisible}>
            <a href="https://www.dados.rio/" aria-label="The Pudding" target="_self">
                {@html logo}
            </a>
        </div>
         
    </div>
    <div class="slider-wrapper">
        {#if $sliderVisible}
            <div id="range-slider" 
                class:isHidden={$readingListVisible}
                in:fade={{ delay: 250, duration: 200 }}
                out:fade={{ delay: 0, duration: 200 }}>
                <!-- <p><Icon name="move-left" /></p> -->
                <div class="range-wrapper">
                    {#if $activeSection == "raunchiness"}
                        <Range value={rangeStart} min={0} max={100} step={1} showTicks={false} section={"raunchiness"} bind:sliderValRaunch /> 
                    {:else if $activeSection == "illustration"}
                        <Range value={rangeStart} min={0} max={100} step={1} showTicks={false} section={"illustration"} bind:sliderValIllo />
                    {:else if $activeSection == "race"}
                        <Range value={rangeStart} min={0} max={100} step={1} showTicks={false} section={"race"} bind:sliderValRace />
                    {/if} 
                </div>
                <!-- <p><Icon name="move-right" /></p> -->
            </div>
        {/if}
    </div>
</nav>

<style>
    nav {
        width: 100%;
        position: fixed;
        top: 0rem;
        padding: 0;
        display: flex;
        flex-direction: column;
        /* z-index: 1000; */
        height: 6rem;
    }
    .loading {
        pointer-events: none;
    }
    .top-nav {
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        padding: 0.5rem 0 0 0;
        margin: 0 0 2rem 0;
        position: fixed;
        width: 100%;
        height: 3rem;
        margin-left: 2rem;
        margin-top: 1rem;
    }
    :global(.book_inList_black) {
        position: absolute;
        top: 0;
        left: 0;
        height: 2rem;
        width: 2rem;
        background-image: url($svg/x.svg);
        background-size: 1rem 1rem;
        background-repeat: no-repeat;
        background-position: center;
        transition: 0.5s background-color linear;
    }
    :global(.book_noList_black) {
        position: absolute;
        top: 0;
        left: 0;
        height: 2rem;
        width: 2rem;
        background-image: url($svg/book-open-black.svg);
        background-size: 1rem 1rem;
        background-repeat: no-repeat;
        background-position: center;
    }
    .isHidden {
        pointer-events: none;
        opacity: 0.125;
        transition: 0.25s linear;
    }
    .slider-wrapper {
        position: fixed;
        top: 93.7%;
        width: 100%;
        height: 3rem;
    }
    .range-wrapper {
        position: relative;
        width: calc(100% - 4rem);
        display: flex;
		flex-direction: row;
    }
    .logo {
        top:0.75rem;
        right: 14rem;
        width: 1px !important;
        transform: scale(1);
        transition: 0.25s linear;
        position: absolute;
        
    }
    .logo:hover {
        transform: scale(1.10);
    }
    .logo a {
        border-bottom: none;
    }
    :global(.logo svg .st0) {
        fill: var(--color-gray-800);
    }
    :global(.logo svg .st1) {
        fill: rgba(0,0,0,0.25);
    }
    .sect-btns {
        margin: 0 auto;
        display: flex;
        flex-direction: row;
        min-width: 10rem;
        justify-content: space-between;
    }
    .sectionBox {
        width: 20px;
        height: 20px;
        background: white;
        opacity: 1;
        transform: scale(0.65);
        transition: 0.25s linear;
    }
    .sectionBox.active {
        transform: scale(1);
    }
    #sectionBox-intro.active {
        border: 3px solid var(--romance-pink);
    }
    #sectionBox-intro.active {
        border: 3px solid var(--romance-pink);
    }
    #sectionBox-raunchiness.active {
        border: 3px solid var(--romance-blue);
    }
    #sectionBox-illustration.active {
        border: 3px solid var(--romance-yellow);
    }
    #sectionBox-race.active {
        border: 3px solid var(--romance-teal);
    }
    #sectionBox-methods.active {
        border: 3px solid var(--romance-pink);
    }
    .sectionBox:hover {
        opacity: 1; 
    }
    #sectionBox-intro {
        border: 3px solid var(--romance-pink-light);
    }
    #sectionBox-raunchiness {
        border: 3px solid var(--romance-blue-light);
    }
    #sectionBox-illustration {
        border: 3px solid var(--romance-yellow-light);
    }
    #sectionBox-race {
        border: 3px solid var(--romance-teal-light);
    }
    #sectionBox-methods {
        border: 3px solid var(--romance-pink-light);
    }
    .label {
        position: absolute;
        top: 2rem;
        left: 50%;
        transform: translate(-50%, -50%);
        transition: 0.125s all linear;
        background-color: white;
        border-radius: 0.25rem;
        padding: 0.25rem;
        box-shadow: -2px 2px 5px  rgba(0,0,0,0.125);
        opacity: 0; 
        text-transform: capitalize;
        font-family: var(--sans-display);
        font-size: var(--14px);
        text-align: center;
    }
    .label.active {
        opacity: 1; 
    }
    .btn-wrapper {
        position: relative;
        margin: -0.5rem 0 0 0;
    }
    .btn-wrapper:hover .label {
        opacity: 1; 
        transform: translate(-50%, -75%);
        z-index: 1000;
    }
    .btn-wrapper:hover .sectionBox {
        transform: scale(1);
    }
    .listBtn {
        position: absolute;
        left: 0.75rem;
        top: 0.5rem;
        /* top: 0.75rem; */
        background: transparent;
        /* z-index: 1000; */
        border-radius: 50%;
        /* height: 5rem; */
        width: 5rem;
        transition: 0.25s linear;
        transform: scale(1);
    }
    .listBtn:hover {
        transform: scale(1.25);
    }
    .count {
        position: absolute;
        top: -1.3rem;
        right: -0.3rem;
        background: var(--romance-pink-light);
        min-width: 1.25rem;
        min-height: 1.25rem;
        border-radius: 50%;
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 0.125rem 0.25rem;
        font-weight: 900;
        font-size: 10px;
        font-family:  var(--sans-display);
        border: 2px solid white;
        box-shadow: 0.25rem 0 1rem  var(--color-gray-300);
        z-index: 1000;
    }
    #range-slider {
		z-index: 1000;
        padding: 0 1rem;
        width: 100%;
        max-width: 60rem;
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        margin-bottom: calc(var(--thumb-width) * 2);
        margin: 0.25rem auto 0 auto;
	}
    #range-slider p {
		font-size: var(--14px);
		font-family: var(--sans-display);
		width: 2rem;
        margin: 0.75rem 0 0 0;
	}
	#range-slider p:first-of-type {
		text-align: right;
		padding: 0 0.5rem 0 0;
	}
	#range-slider p:last-of-type {
		text-align: left;
		padding: 0 0 0 0.5rem
	}
    /* :global(.listBtn.highlight) {
        background: rgba(255, 255, 255, 0.75) !important;
        animation: pulse-animation 2s infinite 1s !important;
    } */
    @keyframes pulse-animation {
        0% {
            box-shadow: 0 0 0 0px rgba(255, 255, 255, 0.75);
        }
        100% {
            box-shadow: 0 0 0 16px rgba(255, 255, 255, 0);
        }
    }
    @keyframes scale-up {
        0% {
            transform: scale(1);
        }
        20% {
            transform: scale(1.5);
        }
    }
    @media only screen and (min-width: 400px) {
        .sect-btns {
            min-width: 15rem;
        }
	}
    @media only screen and (min-width: 400px) and (max-width: 540px){
.logo{
    top:0.8rem;
    right: 12.5rem;
}
.listBtn{
    left: -1rem;
}
    }
</style>