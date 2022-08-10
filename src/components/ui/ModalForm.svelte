<script>
	import { Transition } from '@rgossiaux/svelte-headlessui';
	import { createEventDispatcher } from 'svelte';
	export let show = false;
	export let title = '';

	const dispatch = createEventDispatcher();
</script>

{#if show}
	<Transition
		{show}
		enter="transition-opacity duration-75"
		enterFrom="opacity-0"
		enterTo="opacity-100"
		leave="transition-opacity duration-150"
		leaveFrom="opacity-100"
		leaveTo="opacity-0"
	>
		<div
			class="modal z-50 fixed w-full h-full top-0 left-0 flex items-center justify-center p-8 lg:p-0"
		>
			<div class="modal-overlay fixed w-full h-full bg-gray-900 opacity-50" />
			<div
				class="bg-white w-full lg:h-max lg:w-1/3 mx-auto rounded-lg shadow-xl z-50 overflow-y-auto"
			>
				<div
					class="flex items-center justify-between bg-yellow-100 border-2 border-yellow-700 rounded-t-lg py-2 px-8"
				>
					<h1 class="font-brand font-semibold text-yellow-700 text-xl">{title}</h1>
					<button
						class="p-2 bg-yellow-100 hover:bg-yellow-200 rounded-full ml-4 border-2 border-yellow-700"
						on:click={() => dispatch('close')}
					>
						<svg
							xmlns="http://www.w3.org/2000/svg"
							height="12px"
							viewBox="0 0 24 24"
							width="12px"
							fill="#000000"
							><path d="M0 0h24v24H0V0z" fill="none" /><path
								d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12 19 6.41z"
							/></svg
						>
					</button>
				</div>
				<div class="content p-8 bg-yellow-200 border-2 border-yellow-700 rounded-b-lg">
					<slot name="body" />
				</div>
			</div>
		</div>
	</Transition>
{/if}
