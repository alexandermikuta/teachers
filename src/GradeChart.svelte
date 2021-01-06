<script>
    export let values;
</script>

<style>
    .chart {
        padding: 5px 10px;
    }

    .bar-chart {
        display: inline-block;
        background: navy;
        margin: 0;
        padding: 0;
        transition: all 0.3s ease;
        height: 12px;
    }

    .bar-chart:hover {
        opacity: 0.5;
        cursor: pointer;
    }
</style>

<div class="chart">
    {#each values as value}
        <div>
            <span>Note {value.value}: </span><span
                class="bar-chart"
                style="width: {10 * value.count}px" /><span>{value.count}</span>
        </div>
    {/each}
</div>
