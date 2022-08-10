<script>
	import { onMount } from 'svelte';
	import Resizer from 'react-image-file-resizer';
	import { supabase } from '../lib/subabase';

	const user = supabase.auth.user();

	let files;

	let loading = false;
	let removeLoading = false;
	let addingFile = false;
	let userPhotos = [];

	async function getImageUrls(path_url) {
		const { data, error } = await supabase.storage.from('images').download(path_url);

		if (error) {
			throw error;
		}

		const blob = URL.createObjectURL(data);
		return blob;
	}

	onMount(async () => {
		loading = true;
		try {
			const { data, error } = await supabase
				.from('user_photos')
				.select('photo_url')
				.eq('user_id', user.id);
			if (error) {
				throw error;
			}

			if (data.length > 0) {
				let temp = [];
				for (let i = 0; i <= data.length - 1; i++) {
					const { photo_url } = data[i];
					const blob = await getImageUrls(photo_url);
					temp.push({ file: blob, image_name: photo_url });
				}
				userPhotos = temp;
			}
		} catch (error) {
			console.log(error);
		} finally {
			loading = false;
		}
	});

	const resizeFile = (file) =>
		new Promise((resolve) => {
			Resizer.imageFileResizer(
				file,
				750,
				500,
				'JPEG',
				100,
				0,
				(uri) => {
					resolve(uri);
				},
				'file',
				1800,
				1200
			);
		});

	const handleFileChange = async () => {
		try {
			addingFile = true;
			if (!files || files.length === 0) {
				throw new Error('No files selected');
			}

			const file = files[0];
			const fileExt = file.name.split('.').pop();
			const fileName = `${Math.random()}.${fileExt}`;
			const filePath = `${fileName}`;

			const image = await resizeFile(file);

			let { error: uploadError } = await supabase.storage.from('images').upload(filePath, image);

			if (uploadError) throw uploadError;

			const { error } = await supabase.from('user_photos').insert([
				{
					updated_at: new Date(),
					photo_url: fileName,
					user_id: user.id
				}
			]);

			if (error) throw error;
			window.location.reload();
		} catch (error) {
			console.error(error);
		} finally {
			addingFile = false;
		}
	};

	const handleDeleteFile = async (file_name) => {
		try {
			removeLoading = true;
			const { error } = await supabase.from('user_photos').delete().eq('photo_url', file_name);

			if (error) throw error;

			const { error: DeleteError } = await supabase.storage.from('images').remove([`${file_name}`]);
			if (DeleteError) throw DeleteError;

			window.location.reload();
		} catch (error) {
			console.error(error);
		} finally {
			removeLoading = false;
		}
	};
</script>

<div>
	<div class="flex justify-between items-center w-full">
		{#if !loading}
			<div>
				<h1 class="text-2xl font-brand font-semibold text-gray-700">
					{3 - userPhotos.length} Photos Left to Upload
				</h1>
			</div>
		{/if}
		{#if !loading && userPhotos.length < 3}
			<div>
				<label
					for="dropzone-file"
					class="flex items-center justify-center bg-yellow-100 p-2 font-brand font-semibold rounded text-yellow-700 border-2 border-yellow-700 hover:bg-yellow-200 hover:cursor-pointer"
				>
					{#if addingFile}
						<svg
							role="status"
							class="inline mr-3 w-4 h-4 text-yellow-700 animate-spin"
							viewBox="0 0 100 101"
							fill="none"
							xmlns="http://www.w3.org/2000/svg"
						>
							<path
								d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
								fill="#E5E7EB"
							/>
							<path
								d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
								fill="currentColor"
							/>
						</svg>
					{/if}
					<div>Upload Content</div>
					<input
						id="dropzone-file"
						type="file"
						class="hidden"
						accept="image/*"
						bind:files
						on:change={handleFileChange}
					/>
				</label>
			</div>
		{/if}
	</div>
	<div class="mt-6">
		{#if loading}
			<div class="w-full flex items-center justify-center mx-auto mt-12">
				<svg
					role="status"
					class="inline mr-2 w-10 h-10 text-yellow-700 animate-spin"
					viewBox="0 0 100 101"
					fill="none"
					xmlns="http://www.w3.org/2000/svg"
				>
					<path
						d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
						fill="#E5E7EB"
					/>
					<path
						d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
						fill="currentColor"
					/>
				</svg>
			</div>
		{:else if userPhotos.length > 0}
			<div class="grid grid-cols-3 items-center justify-center gap-1">
				{#each userPhotos as src}
					<div class="flex flex-col items-center justify-center gap-2">
						<img
							src={src.file}
							alt="user uploaded content"
							class="w-80 h-80 object-cover rounded"
						/>
						<button
							disabled={removeLoading}
							on:click={() => handleDeleteFile(src.image_name)}
							class="bg-yellow-100 p-2 font-brand font-semibold rounded text-yellow-700 border-2 border-yellow-700 hover:bg-yellow-200 hover:cursor-pointer"
						>
							{#if removeLoading}
								<svg
									role="status"
									class="inline mr-3 w-4 h-4 text-yellow-700 animate-spin"
									viewBox="0 0 100 101"
									fill="none"
									xmlns="http://www.w3.org/2000/svg"
								>
									<path
										d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
										fill="#E5E7EB"
									/>
									<path
										d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
										fill="currentColor"
									/>
								</svg>
							{/if}
							Remove Photo
						</button>
					</div>
				{/each}
			</div>
		{:else}
			<h1 class="font-brand text-gray-700 mt-4">
				Hey! You have no photos, start uploading some now!
			</h1>
		{/if}
	</div>
</div>
