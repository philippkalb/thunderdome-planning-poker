<script>
    import { createEventDispatcher } from 'svelte'
    import WarriorIcon from './icons/WarriorIcon.svelte'

    const dispatch = createEventDispatcher()

    export let point = '1'
    export let active = false
    export let isLocked = true
    export let results = {
        count: 0,
        voters: [],
    }

    let showVoters = false

    $: activeColor = active
        ? 'border-green-500 bg-green-100 text-green-600'
        : 'border-gray-300 bg-white'
    $: lockedClass = isLocked
        ? 'opacity-25 cursor-not-allowed'
        : 'cursor-pointer'

    function voteAction() {
        if (isLocked) {
            return false
        }
        if (!active) {
            dispatch('voted', {
                point,
            })
        } else {
            dispatch('voteRetraction')
        }
    }
</script>

<div
    data-testId="pointCard"
    data-active="{active}"
    data-locked="{isLocked}"
    data-point="{point}"
    class="relative">
    {#if results.count}
        <div
            class="text-green-500 font-semibold inline-block absolute right-0
            top-0 p-2 text-4xl text-right {showVoters ? 'z-20' : 'z-10'}"
            data-testId="pointCardCount">
            {results.count}
            <button
                on:mouseenter="{() => (showVoters = true)}"
                on:mouseleave="{() => (showVoters = false)}"
                title="Show Voters"
                class="text-green-500 relative leading-none">
                <WarriorIcon height="24" width="24" />
                <span
                    class="text-right text-sm text-gray-900 font-normal w-48
                    absolute left-0 top-0 mt-0 ml-6 bg-white p-2 rounded
                    shadow-lg {showVoters ? '' : 'hidden'}">
                    {#each results.voters as voter}
                        {voter}
                        <br />
                    {/each}
                </span>
            </button>
        </div>
    {/if}
    <div
        class="w-full rounded overflow-hidden shadow-lg border {activeColor}
        {lockedClass} relative text-3xl lg:text-5xl relative z-0"
        on:click="{voteAction}">
        <div class="py-12 md:py-16 text-center">{point}</div>
    </div>
</div>
