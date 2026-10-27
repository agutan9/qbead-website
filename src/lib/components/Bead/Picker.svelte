<!--
@component
Renders an exercise when a bead is paired and its children (normally the original "Try it
yourself" Callout) when not. Lets one lesson work with and without hardware.

Props:
- `exercise`: the exercise data (see `$lib/bead/exercises`).
- children: the fallback shown when no bead is paired.

```svelte
<BeadExercise exercise={gatesXY}>

<Callout type="question" title="Try it yourself">

1. Apply X ...

</Callout>

</BeadExercise>
```
-->
<script lang="ts">
  import type { Snippet } from 'svelte'
  import Callout from '$lib/components/Callout/Callout.svelte'

  let pressedOption = $state<string | null>(null)
  let {
    title,
    options,
    correctAnswer,
    children,
  }: { title: string; options: string[]; correctAnswer: number; children?: Snippet } = $props()
</script>

<Callout type="question" {title}>
  <div class="grid grid-cols-1 items-start gap-4 md:grid-cols-1">
    {@render children?.()}
    <div class="flex flex-col gap-4">
      <div class="flex flex-wrap gap-4">
        {#each options as op (op)}
          <button
            class="btn {pressedOption === op
              ? op === options[correctAnswer]
                ? 'preset-tonal-primary'
                : 'preset-filled-warning-500'
              : 'preset-filled-primary-500'}"
            onclick={() => (pressedOption = op)}
          >
            {op}
          </button>
        {/each}
      </div>
      {#if pressedOption}
        <p>
          {pressedOption === options[correctAnswer]
            ? 'Correct! Well done.'
            : `Incorrect. Try again!`}
        </p>
      {/if}
    </div>
  </div>
</Callout>

<!--
{#if bead.status === 'live'}
  <Callout type="question" title={exercise.title}>
    <div class="grid grid-cols-1 items-start gap-6 md:grid-cols-2">
      <BeadSphere />
      <div class="flex flex-col gap-4">
        {#each exercise.widgets as widget, i (i)}
          {#if typeof widget !== 'string'}
            {@const W = widget}
            <W />
          {:else if widget === 'picker'}
            <GatePicker />
          {:else if widget === 'history'}
            <GateHistory />
          {:else if widget === 'readout'}
            <KetReadout />
          {:else if widget === 'measure'}
            <MeasureCounter />
          {:else if widget === 'tasks' && exercise.tasks}
            <TaskList tasks={exercise.tasks} />
          {:else if widget === 'challenges' && exercise.challenges}
            <ChallengeRunner challenges={exercise.challenges} />
          {/if}
        {/each}
      </div>
    </div>
  </Callout>
{:else}
  {@render children?.()}
{/if}
-->
