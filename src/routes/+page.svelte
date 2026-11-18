<script lang="ts">
    import {onMount} from 'svelte';
    import Chart from 'chart.js/auto';
    import {goto} from '$app/navigation'
    import { resolve}  from '$app/paths';

    const test = [
        {id: 1, date: '2026-11-11', mood: 6},
        {id: 2, date: '2026-11-12', mood: 2},
        {id: 3, date: '2026-11-13', mood: 8}
    ]

    let canvas: HTMLCanvasElement;
    let clicked = $state<string | null>(null);

    onMount(() => {
        const chart = new Chart(canvas, {
            type: 'line',
            data: {
                labels: test.map((e) => e.date),
                datasets: [
                    {
                        label: 'Mood',
                        data: test.map((e) => e.mood),
                        tension: 0.3,
                        pointRadius: 5,
                        pointHoverRadius: 10,
                    }
                ]
            },
            options: {
                maintainAspectRatio: false,
                scales: { y : { min: 1, max: 10}},
                onClick: (_event, elements) => {
                    if (elements.length) {
                        const entry = test[elements[0].index];
                        goto(`/journal/${entry.date}`)
                    }
                },
                onHover: (event, elements) => {
                    (event.native?.target as HTMLElement).style.cursor = elements.length ? 'pointer' : 'default';
                }
            }
        })

        return () => {
            chart.destroy();
        }
    })
</script>


<div class="relative h-72">
    <canvas bind:this={canvas}></canvas>
</div>
<p class="mt-4 text-sm">{clicked ?? 'Click a point on the graph'}</p>
