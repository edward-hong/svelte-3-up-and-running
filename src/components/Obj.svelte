<script>
  import Renderer from './Renderer.svelte'
  import ErrorBox from './ErrorBox.svelte'
  import { LoadObject } from '../lib/Requests'
  import { token } from '../stores'

  export let objectId = null

  let contentPromise = Promise.resolve({})

  $: contentPromise = LoadObject(objectId, $token)
</script>

{#await contentPromise}
  Loading...
{:then response}
  <p class="mb-2 italic">
    Saved on {response && response.date ? new Date(response.date).toLocaleString() : '(null)'}
  </p>
  <Renderer
    title={(response && response.title) || ''}
    content={(response && response.content) || ''} />
{:catch err}
  <ErrorBox {err} />
{/await}
