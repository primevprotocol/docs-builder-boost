<script>
  import '@svelteness/kit-docs/client/polyfills/index.js';
  import '@svelteness/kit-docs/client/styles/normalize.css';
  import '@svelteness/kit-docs/client/styles/fonts.css';
  import '@svelteness/kit-docs/client/styles/theme.css';
  import '@svelteness/kit-docs/client/styles/vars.css';


  import { page } from '$app/stores';
  import PrimevLogo from '$img/favicon.png';

  import { Button, KitDocs, KitDocsLayout, createSidebarContext, SocialLink } from '@svelteness/kit-docs';

  /** @type {import('./$types').LayoutData} */
  export let data;

  $: ({ meta, sidebar } = data);

  /** @type {import('@svelteness/kit-docs').NavbarConfig} */
  const navbar = {
    links: [
      // { title: 'Documentation', slug: '/docs', match: /\/docs/ }
    ],
  };

  const { activeCategory } = createSidebarContext(sidebar);

  $: category = $activeCategory ? `${$activeCategory}: ` : '';
  $: title = meta ? `${category}${meta.title}` : null;
  $: description = meta?.description;
</script>

<svelte:head>
  {#key $page.url.pathname}
    {#if title}
      <title>{title}</title>
    {/if}
    {#if description}
      <meta name="description" content={description} />
    {/if}
  {/key}
</svelte:head>

<KitDocs {meta}>
  <KitDocsLayout {navbar} {sidebar}>
    <div class="logo" slot="navbar-left">
      <Button href="/">
        <img src={PrimevLogo} alt="primev" width="100"/>
      </Button>
    </div>

    <slot />
    <div slot="navbar-right-alt">
      <div class="flex items-center">
        <SocialLink type="twitter" href="//twitter.com/primev_xyz" class="socialIcon" />
        <SocialLink type="gitHub" href="//github.com/primevprotocol" class="socialIcon" />
      </div>
    </div>
  </KitDocsLayout>
</KitDocs>

<style>
  :global(:root) {
    --kd-color-brand-rgb: 233, 127, 6;
  }

  :global(:root.dark) {
    --kd-color-brand-rgb: 213, 149, 76;
  }

  .logo :global(a) {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .logo :global(svg) {
    height: 36px;
    overflow: hidden;
  }
</style>
