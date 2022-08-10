<script>
	import '../app.css';
	import NavbarTopLink from '../components/NavbarTopLink.svelte';
	import { user } from '$lib/sessionStore';

	import { supabase } from '$lib/subabase';
	import { onMount } from 'svelte';

	let loading = true;

	onMount(() => {
		try {
			user.set(supabase.auth.user());
		} catch (error) {
			console.log(error);
		} finally {
			loading = false;
		}
	});

	supabase.auth.onAuthStateChange((_, session) => {
		try {
			loading = true;
			user.set(session?.user);
		} catch (error) {
			console.log(error);
		} finally {
			loading = false;
		}
	});

	const links = [{ linkName: 'Login', linkUrl: '/log-in' }];
</script>

<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
	<link
		href="https://fonts.googleapis.com/css2?family=Work+Sans:ital,wght@0,300;0,400;0,500;0,600;0,700;0,800;1,300;1,400;1,500;1,600;1,700;1,800&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<div>
	<div class="border-b border-gray-100">
		<div class="max-w-4xl lg:mx-auto mx-2">
			<div class="flex items-center justify-between w-full py-4">
				<div class="flex items-center gap-2">
					<a href="/" class="flex items-center gap-2 text-xl font-bold text-gray-700 font-brand"
						>MemoryPrints<span
							class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-semibold bg-yellow-100 text-yellow-800 font-brand"
						>
							BETA
						</span></a
					>
					<div class="space-x-2 font-medium text-sm text-gray-600">
						<NavbarTopLink linkName="Service" linkUrl="/service" />
						<NavbarTopLink linkName="About Us" linkUrl="/about" />
					</div>
				</div>
				<div>
					<div class="space-x-2 font-medium text-sm text-gray-600">
						{#if !$user}
							{#each links as link}
								<NavbarTopLink linkName={link.linkName} linkUrl={link.linkUrl} />
							{/each}
						{:else}
							<NavbarTopLink linkName="Dashboard" linkUrl="/dashboard" />
						{/if}
					</div>
				</div>
			</div>
		</div>
	</div>
	<div class="mt-12 max-w-4xl mx-2 lg:mx-auto selection:bg-yellow-100">
		{#if loading}
			<div />
		{:else}
			<slot />
		{/if}
	</div>
</div>
