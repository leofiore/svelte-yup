<script>
    import {getContext} from 'svelte';
    import {schemaKey, fieldsKey, submittedKey, colorKey} from './key';
    let schema = getContext(schemaKey);
    let fields = getContext(fieldsKey);
    let submitted = getContext(submittedKey);
    let color = getContext(colorKey);
</script>
  
{#if $submitted}
  {#await $schema.validate($fields, {abortEarly: false}) then result}
    <slot name="valid">
      <p></p>
    </slot>
    {:catch errors}
      {#each errors.errors as error}
        <slot {error}>
          <p class="invalid" style="color:{$color}">{error}</p>
        </slot>
      {/each}
  {/await}
{/if}
  
<style>
  p.invalid {
      color: red;
      margin: 0px;
      font-size: 14px;
  }
</style>
