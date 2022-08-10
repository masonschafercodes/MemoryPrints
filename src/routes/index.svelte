<svelte:head>
	<title>MemoryPrints | Home</title>
</svelte:head>

<div class="text-center mt-24">
	<div>
		<h1 class="font-brand text-4xl text-gray-600 font-bold">
			Welcome to <span class="underline decoration-yellow-200 decoration-4">MemoryPrints</span>
		</h1>
		<p class="mt-2 font-brand text-gray-500">
			Lorem ipsum dolor sit amet consectetur adipisicing elit. Praesentium debitis aspernatur dolor,
			vero provident minima at fuga nisi animi voluptate ad voluptas quod veniam similique
			accusamus, ipsa voluptates! Quasi, corrupti.
		</p>
		<div class="mt-6">
			<a
				href="/sign-up"
				class="bg-yellow-100 text-yellow-700 hover:bg-yellow-200 px-4 py-3 font-brand text-semibold rounded font-semibold border-2 border-yellow-700"
			>
				Get started
			</a>
		</div>
	</div>
	<div class="mt-48">
		<h1 class="font-brand text-2xl text-gray-600 font-bold">How Does MemoryPrints Work?</h1>
		<div class="flex items-center justify-center space-x-2">
			<div class="mt-6 flex-1">
				<h4 class="font-brand text-lg text-gray-600 font-bold">Upload your Photos</h4>
				<p class="font-brand text-gray-500 mt-2">
					Lorem ipsum dolor sit amet consectetur adipisicing elit.
				</p>
			</div>
			<div class="mt-6 flex-1">
				<h4 class="font-brand text-lg text-gray-600 font-bold">Choose your Recipients</h4>
				<p class="font-brand text-gray-500 mt-2">
					Lorem ipsum dolor sit amet consectetur adipisicing elit.
				</p>
			</div>
			<div class="mt-6 flex-1">
				<h4 class="font-brand text-lg text-gray-600 font-bold">Photos. Delivered.</h4>
				<p class="font-brand text-gray-500 mt-2">
					Lorem ipsum dolor sit amet consectetur adipisicing elit.
				</p>
			</div>
		</div>
	</div>
	<div class="my-48">
		<div class="flex items-center gap-6">
			<div class="flex-1 hidden lg:block">
				<img src="https://i.ibb.co/JnrqHdc/happy-old-people.png" alt="happy-old-people" />
			</div>
			<div class="flex-1 lg:text-left">
				<h1 class="font-brand text-xl text-gray-600 font-bold">
					Make a family member happy for as little <br /> as $4.99
				</h1>
				<p class="font-brand text-gray-500 mt-2">
					Lorem ipsum dolor sit amet consectetur adipisicing elit. Ratione, veritatis quae. Fuga
					iste ducimus distinctio id. Magnam excepturi eaque quae.
				</p>
				<div class="mt-6 space-y-2">
					<p class="font-brand font-semibold text-gray-500">
						3 high quality 4x6 photos printed and delivered
					</p>
					<p class="font-brand font-semibold text-gray-500">
						Choose who recieves the photos each month
					</p>
					<p class="font-brand font-semibold text-gray-500">Delivered anywhere in the US</p>
				</div>
			</div>
		</div>
	</div>
	<div class="mb-6">
		<footer class="flex flex-col gap-4 items-center justify-center">
			<p class="font-medium font-brand text-gray-600">
				&copy; MemoryPrints {new Date().getFullYear()}
			</p>
			<div class="flex gap-6">
				<a href="/privacy" class="font-brand text-gray-600 hover:underline">Privacy</a>
				<a href="/terms-of-service" class="font-brand text-gray-600 hover:underline">Terms</a>
				<a href="/support" class="font-brand text-gray-600 hover:underline">Support</a>
			</div>
		</footer>
	</div>
</div>
