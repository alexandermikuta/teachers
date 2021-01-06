<script>
    import PointsButton from "./PointsButton.svelte";
    import Chart from "./GradeChart.svelte";

    let grades = [
        { value: 1, from: 50, to: 60 },
        { value: 2, from: 40, to: 49.5 },
        { value: 3, from: 30, to: 39.5 },
        { value: 4, from: 20, to: 29.5 },
        { value: 5, from: 10, to: 19.5 },
        { value: 6, from: 0, to: 9.5 },
    ];
    let points = [];

    function getGrade(tmpGrades, points) {
        let matchingGrade = tmpGrades.find(
            (g) => g.from <= points && g.to >= points
        );
        return matchingGrade ? matchingGrade.value : undefined;
    }

    function getPointSteps(pointRange) {
        let steps = [];
        for (let i = pointRange.from; i <= pointRange.to; i += 0.5) {
            steps.push(i);
        }

        return steps;
    }

    function plus(pointStep) {
        points = [...points, pointStep];
    }

    function minus(pointStep) {
        const index = points.indexOf(pointStep);

        points = [...points.slice(0, index), ...points.slice(index + 1)];
    }

    $: pointRange = {
        from: Math.min(...grades.map((g) => g.from)),
        to: Math.max(...grades.map((g) => g.to)),
    };
    $: notes = points.map((p) => getGrade(grades, p));
    $: notesSum = notes.reduce((pv, cv) => pv + cv, 0);
    $: average = parseFloat(
        Math.round((notesSum / notes.length) * 100) / 100
    ).toFixed(2);
    $: pupilCount = notes.length;
    $: gradeCounts = grades.map((g) => ({
        value: g.value,
        count: notes.filter((n) => n === g.value).length,
    }));
</script>

<style>
    .container {
        display: grid;
        grid-template-columns: 560px auto;
    }

    .points-list {
        display: grid;
        gap: 16px;
        row-gap: 24px;
        grid-template-columns: repeat(auto-fit, minmax(80px, 90px));
    }
</style>

<div class="container">
    <div class="left">
        <div>
            {#each grades as grade}
                <p>
                    <b>{grade.value}:</b>
                    <input placeholder="Von" bind:value={grade.from} />
                    <input placeholder="Bis" bind:value={grade.to} />
                </p>
            {/each}
        </div>
        {#if points.length > 0}
        <Chart values={gradeCounts} />
        <div>&Oslash; {average} ({pupilCount} Schüler)</div>
        {/if}
    </div>

    <div class="points-list">
        {#each getPointSteps(pointRange) as pointStep}
            <PointsButton
                value={pointStep}
                count={points.filter((p) => p === pointStep).length}
                on:minus={minus(pointStep)}
                on:plus={plus(pointStep)} />
        {/each}
    </div>
</div>
