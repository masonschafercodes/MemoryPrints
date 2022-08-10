<script>
	import SignupForm from '../components/SignupForm.svelte';
	import { supabase } from '../lib/subabase';

	let loading = false;
	let signupError = null;

	async function handleSignup(event) {
		try {
			loading = true;
			const { user, error } = await supabase.auth.signUp({ ...event.detail });
			if (user) {
				window.location.href = '/dashboard';
			} else if (error) {
				throw error;
			}
		} catch (error) {
			signupError = error.message;
		} finally {
			loading = false;
		}
	}
</script>

<svelte:head>
	<title>MemoryPrints | Sign up</title>
</svelte:head>

<div class="flex flex-col items-center justify-center w-full">
	<div>
		<h1 class="font-brand text-2xl text-gray-600 font-semibold">Sign up to MemoryPrints</h1>
	</div>
	<div class="w-1/2">
		<SignupForm on:submit={handleSignup} {loading} error={signupError} authButtonText="Sign Up" />
	</div>
</div>
