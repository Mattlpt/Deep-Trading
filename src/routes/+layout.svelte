<script>
  import { ModeWatcher } from 'mode-watcher'
  import { page } from '$app/stores'
  import '../app.pcss'

  import { invalidate } from '$app/navigation'
  import { onMount } from 'svelte'

  export let data
  
  const title = $page.data?.title

  let { supabase, session } = data
  $: ({ supabase, session } = data)

  onMount(() => {
    const { data } = supabase.auth.onAuthStateChange((event, _session) => {
      if (_session?.expires_at !== session?.expires_at) {
        invalidate('supabase:auth')
      }
    })

    return () => data.subscription.unsubscribe()
  })
</script>

<svelte:head>
  <title>Deep Trading{title ? ` • ${title}` : ''}</title>
</svelte:head>

<ModeWatcher />
<slot />
