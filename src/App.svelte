<script lang="ts">
  import { Canvas } from '@threlte/core'
  import Scene from './Scene.svelte'
  import { beforeUpdate, tick } from 'svelte';

  let list = [], target = undefined

  const waitLoad = new Promise(resolve => {
    setTimeout(() => {
      resolve(list)
    }, 2000)
  })

  let open = true
</script>

{#await waitLoad then list}
<div class="absolute h-100vh w-100 p-4 flex flex-col">
  <div>
    <button class="bg-white rounded-xl p-2" on:click={() => {
      open = !open
    }}>Open / Close</button>
  </div>
  {#if open}
  <div class="w-full h-full rounded-xl flex flex-col gap-1 items-start p-1 overflow-x-hidden overflow-y-auto">
    {#each list.children[1].children[1].children as node}
    <button class="bg-white rounded-xl w-full py-2" on:click={() => {
      target = node
    }}>{node.name}</button>
    {/each}
  </div>
  {/if}
</div>
{/await}

<div class="h-100vh w-full">
  <Canvas>
    <Scene bind:list bind:target />
  </Canvas>
</div>