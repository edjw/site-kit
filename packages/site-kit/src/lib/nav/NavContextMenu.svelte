<script>
	import { page } from '$app/stores';
	import { onMount } from 'svelte';

	/**
	 * @type {import('../types').Section[]}
	 */
	export let contents = [];

	/** @type {HTMLElement} */
	let nav;

	onMount(() => {
		scrollToActive();
	});

	export async function scrollToActive() {
		const active = /** @type {HTMLElement} */ (nav.querySelector('[aria-current="true"]'));
		if (!active) return;

		const parent = nav.parentElement;
		if (!parent) return;

		const parentCenter = parent.offsetHeight / 2;
		const childCenter = active.offsetHeight / 2;
		const offsetTop = active.offsetTop;
		const scrollPosition = offsetTop - parentCenter + childCenter;

		const updateScroll = () => (parent.scrollTop = scrollPosition);

		requestAnimationFrame(updateScroll);
	}
</script>

<nav bind:this={nav}>
	{#each contents as { sections, title }}
		<section>
			<h3>{title}</h3>

			<ul>
				{#each sections as { path, title, badge }}
					<li>
						<a href={path} aria-current={path === $page.url.pathname}>
							{title}

							{#if badge}
								<span style="flex: 1 1 auto" />
								<span class="badge">{badge}</span>
							{/if}
						</a>
					</li>
				{/each}
			</ul>
		</section>
	{/each}
</nav>

<style>
	nav {
		padding: 2rem;
		font-family: var(--sk-font);
		overflow-y: auto;
	}

	h3 {
		display: block;
		padding-bottom: 0.8rem;
		font-size: var(--sk-text-xs);
		text-transform: uppercase;
		letter-spacing: 0.1em;
		font-weight: 600;
		color: var(--sk-text-3);
	}

	ul {
		list-style-type: none;
		margin: 0;
		margin-bottom: 2.5rem;
	}

	li {
		display: block;
	}

	a {
		display: flex;
		align-items: center;
		border-radius: var(--sk-border-radius);
		line-height: 1;
		color: var(--sk-text-2);
		padding: 0.9rem 0.75rem !important;
		transition: 0.1s ease;
		transition-property: background-color, color;
	}

	a:hover {
		text-decoration: none;
		background-color: var(--sk-back-4);
	}

	[aria-current='true'] {
		background-color: hsla(var(--sk-theme-1-hsl), 0.1) !important;
		color: var(--sk-theme-1) !important;
		font-weight: 400;
	}

	.badge {
		display: flex;
		justify-content: center;
		align-items: center;
		padding: 0.5rem 0.75rem;
		border-radius: 30px;
		font-size: 1.1rem;
		font-weight: 600;
		letter-spacing: 1px;
		font-family: var(--sk-font);
		line-height: 1;
		color: var(--sk-theme-1);
		background: hsla(var(--sk-theme-1-hsl), 0.1);
	}
</style>
