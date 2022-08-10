<script>
	export let person;
</script>

<div
	class="bg-yellow-100 p-2 border-2 border-yellow-700 text-yellow-700 rounded font-brand text-sm"
>
	<h2 class="font-semibold">{person.name}</h2>
	<div class="mt-2 flex flex-col">
		<p>{person.address.street}</p>
		<p>{person.address.city}, CA {person.address.zipcode}</p>
		<p>United States</p>
	</div>
</div>
