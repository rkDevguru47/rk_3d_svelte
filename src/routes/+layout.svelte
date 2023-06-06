<script lang='ts'>
	// The ordering of these imports is critical to your app working properly
	import '../theme.postcss';
	// If you have source.organizeImports set to true in VSCode, then it will auto change this ordering
	import '@skeletonlabs/skeleton/styles/skeleton.css';
	// Most of your app wide CSS should be put in this file
	import '../app.postcss';
	import { AppShell, AppBar, LightSwitch } from '@skeletonlabs/skeleton';
</script>

<!-- App Shell -->
<AppShell>
	<svelte:fragment slot="header">
		<!-- App Bar -->
		<AppBar>
			<svelte:fragment slot="lead">
				<strong class="text-xl uppercase">3D - with Threlte</strong>
			</svelte:fragment>
			<svelte:fragment slot="trail">
				<a
					class="btn btn-sm variant-ghost-success"
					href="/spooky"
					rel="noreferrer"
				>
					Spooky
				</a>
				<a
				class="btn btn-sm variant-ghost-error"
				href="/"
				rel="noreferrer"
			>
				Sphere
			</a>
				<a
					class="btn btn-sm variant-ghost-tertiary"
					href="https://rishu47.vercel.app/"
					target="_blank"
					rel="noreferrer"
				>
					Rishu Kumar
				</a>
				<a
					class="btn btn-sm variant-ghost-primary"
					href="https://github.com/rkDevguru47"
					target="_blank"
					rel="noreferrer"
				>
					GitHub
				</a>
				<LightSwitch/>
			</svelte:fragment>
		</AppBar>
	</svelte:fragment>
	<!-- Page Route Content -->
	<slot />
</AppShell>
