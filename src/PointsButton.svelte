<script>
    import { createEventDispatcher } from "svelte";

    export let value;
    export let count;

    const dispatch = createEventDispatcher();

    const plus = () => {
        dispatch("plus");
    };

    const minus = () => {
        dispatch("minus");
    };
</script>

<style>
    .button-group {
        width: 80px;
        border: 1px solid navy;
        white-space: nowrap;
        text-align: center;
    }

    .button {
        width: 27px;
        height: 32px;
        padding: 0;
        margin: 0;
    }

    .count {
        padding: 0 5px;
    }

    .value {
        color: white;
        background: navy;
        height: 16px;
        font-weight: bold;
    }
</style>

<div class="button-group">
    <div class="counter">
        <input
            class="button"
            on:click={minus}
            type="button"
            value="-"
            disabled={count === 0} />
        <span class="count">{count}</span>
        <input class="button" on:click={plus} type="button" value="+" />
    </div>
    <div class="value">{value}</div>
</div>
