<script>
    import { onMount } from 'svelte';
    import { displayTime } from '../../Services/Utils';

    let total = 28;
    let seconds = 0;

    $: percent = 0;
    $: radius = 44;
    $: circumference = radius * 2 * Math.PI;
    $: offset = circumference - (percent / 100) * circumference;
    $: timeLeft = '';
    // $: rotation = (percent * 360) / 100;

    onMount(() => {
        const interval = setInterval(() => {
            seconds += 1;
            percent = (seconds / total) * 100;
            timeLeft = displayTime(total - seconds);
            if (percent > 99) {
                percent = 100;
                clearInterval(interval);
            }
        }, 1000);

        return () => {
            clearInterval(interval);
        };
    });
</script>

<style src="./style.scss">

</style>

<svg viewBox="-50 -50 100 100">
    <g>

        <circle class="circle-face" r="44" />
        <circle
            class="circle-face indicator"
            r="44"
            style="stroke-dasharray: {circumference}
            {circumference}; stroke-dashoffset: {offset}" />
    </g>

    <g class="timer">
        <text dominant-baseline="middle" text-anchor="middle">{timeLeft}</text>
    </g>
</svg>
