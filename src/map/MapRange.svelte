<script>
    export let value = 5;
    let bubble;
    const 
        min = 5, 
        max = 65;

    const handleInputChange = (value, bubble) => {
        if (!bubble) return;
        const relativeValue = Math.max(Math.min((value - min), 65), 0);
        const percentage = relativeValue / (max - min) * 100;
        console.log("Bubble", {bubble});
        bubble.style.left = `${percentage}%`;
        bubble.style.transform = `translateX(-${percentage}%)`;
        console.log("percents", percentage);
    }

    $: handleInputChange(value, bubble);
</script>

<div class="wrapper">    
    <div class="bubble-wrapper">    
        <div bind:this={bubble} class="bubble">{value} km</div>
    </div>
    <input type="range" min={min} max={max} bind:value={value}>
    <div class="descriptions">
        <span>{min} km</span>
        <span>{max} km</span>
    </div>
</div>


<style>
    .bubble-wrapper {
        padding: 1.5em;
        position: relative;
        /* background: #0001; */
    }
    .bubble {
        position: absolute;
        top: 0;
        padding: 12px;
        background: #2767c6;
        transform: translate(0%, -100%);
        border-radius: 12px;
        color: white;
        min-width: 75px;
        text-align: center;
    }
    .wrapper {
        /* position: relative; */
        display: flex;
        align-items: stretch;
        flex-direction: column;
        padding: .5em 1em;
        /* margin-top: 3em; */
    }
    .descriptions {
        display: flex;
        justify-content: space-between;
    }
</style>