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
  import type { ExercisePicker, Exercise } from '$lib/bead/exercises/types'

  let pressedOption = $state<string | null>(null)
  let { exercise, children }: { exercise: ExercisePicker; children?: Snippet } = $props()
  let randAnswer = $state(Math.floor(Math.random() * 2))
</script>

<Callout type="question" title={exercise.title}>
  <div class="grid grid-cols-1 items-start gap-4 md:grid-cols-1">
    <p>{exercise.question}</p>
    <div class="flex flex-col gap-4">
      <div class="flex flex-wrap gap-4">
        {#each exercise.options as op (op)}
          <button
            class="btn {pressedOption === op
              ? op === exercise.options[exercise.correctAnswer]
                ? 'preset-tonal-primary'
                : 'preset-filled-warning-500'
              : 'preset-filled-primary-500'}"
            onclick={() => {
              pressedOption = op
              randAnswer = Math.floor(Math.random() * 2)
            }}
          >
            {op}
          </button>
        {/each}
      </div>
      {#if pressedOption === exercise.options[exercise.correctAnswer]}
        <p>
          {'Correct! Well done.\n' + exercise.justification}
        </p>
      {:else if pressedOption !== null}
        <p>
          {randAnswer === 0 ? 'Incorrect. Try again!' : 'Incorrect. Keep trying!'}
        </p>
      {/if}
    </div>
  </div>
</Callout>
