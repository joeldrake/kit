<script>
	import '@sveltejs/site-kit/styles/index.css';
	import { browser } from '$app/environment';
	import { page, navigating } from '$app/stores';
	import { Icon, Icons, Nav, NavItem, PreloadingIndicator, SkipLink } from '@sveltejs/site-kit';
	import Search from '$lib/search/Search.svelte';
	import SearchBox from '$lib/search/SearchBox.svelte';
	import Logo from './home/svelte-logo.svg';
	import Banner from './Banner.svelte';
</script>

<Icons />

{#if $navigating}
	<PreloadingIndicator />
{/if}

<SkipLink href="#main" />

<Banner />

<Nav {page} logo={Logo}>
	<svelte:fragment slot="nav-center">
		{#if $page.url.pathname !== '/search'}
			<!-- the <Nav> component renders this content inside a <ul>, so
				we need to wrap it in an <li>. TODO if we adopt this design
				on other sites, change <Nav> so we don't need to do this -->
			<li><Search /></li>
		{/if}
	</svelte:fragment>

	<svelte:fragment slot="nav-right">
		<NavItem selected={$page.url.pathname.startsWith('/docs') || undefined} href="/docs"
			>Docs</NavItem
		>
		<NavItem selected={$page.url.pathname.startsWith('/faq') || undefined} href="/faq">FAQ</NavItem>

		<li aria-hidden="true"><span class="separator" /></li>

		<NavItem external="https://svelte.dev">Svelte</NavItem>

		<NavItem external="https://svelte.dev/chat" title="Discord Chat">
			<span class="small">Discord</span>
			<span class="large"><Icon name="message-square" /></span>
		</NavItem>

		<NavItem external="https://github.com/sveltejs/kit" title="GitHub Repo">
			<span class="small">GitHub</span>
			<span class="large"><Icon name="github" /></span>
		</NavItem>
	</svelte:fragment>
</Nav>

<main id="main">
	<slot />
</main>

{#if browser}
	<SearchBox />
{/if}

<style>
	main {
		position: relative;
		margin: 0 auto;
		padding-top: var(--sk-nav-height);
		overflow: hidden;
	}

	.small {
		display: inline;
	}

	.large {
		display: none;
	}

	/* duplicating content from <Nav> — bit hacky but will do for now */
	.separator {
		display: block;
		position: relative;
		height: 1px;
		margin: 0.5rem 0;
		background: radial-gradient(circle at center, rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.05));
	}

	@media (min-width: 800px) {
		.small {
			display: none;
		}

		.large {
			display: inline;
		}

		.separator {
			display: flex;
			align-items: center;
			justify-content: center;
			background: none;
			height: 100%;
			margin: 0;
			border: none;
			text-align: center;
		}

		.separator::before {
			content: '•';
			margin: 0 0.3rem;
			color: #ccc;
		}
	}

	@media (min-width: 960px) {
		/* this is an unfortunate hack, but necessary to temporarily avoid
		breaking changes to site-kit */
		:global(ul.external) {
			width: 30rem !important;
		}
	}

	:global(body) {
		font-size: 1.6rem !important;
	}

	li {
		display: flex;
		align-items: center;
	}

	:global(.examples-container, .repl-outer, .tutorial-outer) {
		height: calc(100vh - var(--sk-nav-height)) !important;
	}

	:global(.toggle) {
		bottom: 0 !important;
	}

	@media (max-width: 830px) {
		:global(aside) {
			z-index: 9999 !important;
		}
	}
</style>
