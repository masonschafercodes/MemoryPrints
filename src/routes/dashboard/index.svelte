<script>
	import { supabase } from '$lib/subabase';
	import { user } from '$lib/sessionStore';
	import SignupForm from '../../components/SignupForm.svelte';
	import Photos from '../../components/Photos.svelte';

	let loading = false;
	let authError = null;

	async function handleSignout() {
		try {
			const { error } = await supabase.auth.signOut();
			if (error) {
				throw error;
			}
		} catch (error) {
			console.log(error);
		}
	}

	async function handleAuth(event) {
		try {
			loading = true;
			const { error } = await supabase.auth.signIn({ ...event.detail });
			if (error) {
				throw error;
			}
		} catch (error) {
			authError = error.message;
		} finally {
			loading = false;
		}
	}
</script>

{#if $user}
	<div>
		<div>
			<Photos />
		</div>
	</div>
{:else}
	<div class="flex flex-col items-center justify-center w-full">
		<div>
			<h1 class="font-brand text-2xl text-gray-600 font-semibold">Sign In to MemoryPrints</h1>
		</div>
		<div class="w-1/2">
			<SignupForm on:submit={handleAuth} {loading} error={authError} />
		</div>
	</div>
{/if}
