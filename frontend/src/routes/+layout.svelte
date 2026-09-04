<script lang="ts">
	import type { Pathname } from '$app/types';
	import { resolve } from '$app/paths';
	import { page } from '$app/state';
	import { locales, localizeHref } from '$lib/paraglide/runtime';
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';

	const navigation: { href: Pathname; label: string; icon: string }[] = [
		{ href: '/', label: 'Home', icon: 'home' },
		{ href: '/demo', label: 'Explore', icon: 'compass' },
		{ href: '/demo/paraglide', label: 'Language', icon: 'globe' },
		{ href: '/demo/playwright', label: 'Practice', icon: 'bolt' }
	];

	let { children } = $props();
</script>

<svelte:head><link rel="icon" href={favicon} /></svelte:head>

<div class="app-shell">
	<main class="page-content">
		{@render children()}
	</main>

	<nav class="bottom-nav" aria-label="Main navigation">
		<div class="bottom-nav__inner">
			{#each navigation as item (item.href)}
				{@const href = resolve(item.href)}
				<a class:active={page.url.pathname === href} href={href}>
				<!-- <a class:active={page.url.pathname === href || (item.href !== '/' && page.url.pathname.startsWith(href + '/'))} href={href}> -->
					<span class="nav-icon" aria-hidden="true">
						{#if item.icon === 'home'}
							<svg viewBox="0 0 24 24"><path d="m3 10 9-7 9 7v10a1 1 0 0 1-1 1h-5v-6H9v6H4a1 1 0 0 1-1-1V10Z" /></svg>
						{:else if item.icon === 'compass'}
							<svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="9" /><path d="m15.5 8.5-2.1 4.9-4.9 2.1 2.1-4.9 4.9-2.1Z" /></svg>
						{:else if item.icon === 'globe'}
							<svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="9" /><path d="M3 12h18M12 3c2.2 2.4 3.3 5.4 3.3 9s-1.1 6.6-3.3 9c-2.2-2.4-3.3-5.4-3.3-9S9.8 5.4 12 3Z" /></svg>
						{:else}
							<svg viewBox="0 0 24 24"><path d="m13 2-9 12h7l-1 8 9-12h-7l1-8Z" /></svg>
						{/if}
					</span>
					<span>{item.label}</span>
				</a>
			{/each}
		</div>
	</nav>
</div>

<div style="display:none">
	{#each locales as locale (locale)}
		<a href={resolve(localizeHref(page.url.pathname, { locale }) as Pathname)}>{locale}</a>
	{/each}
</div>
