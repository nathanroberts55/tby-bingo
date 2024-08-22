<script>
	import { onMount } from 'svelte';

	let phrases = [];
	let cells = [];
	let loading = true; // Add a loading state
	let letters = ['B', 'I', 'N', 'G', 'O'];
	let showModal = false; // Add a state to control the modal visibility
	let winningPositions = [
		[0, 1, 2, 3, 4],
		[5, 6, 7, 8, 9],
		[10, 11, 12, 13, 14],
		[15, 16, 17, 18, 19],
		[20, 21, 22, 23, 24],
		[0, 5, 10, 15, 20],
		[1, 6, 11, 16, 21],
		[2, 7, 12, 17, 22],
		[3, 8, 13, 18, 23],
		[4, 9, 14, 19, 24],
		[0, 6, 12, 18, 24], // Diagonal from top-left to bottom-right
		[4, 8, 12, 16, 20] // Diagonal from top-right to bottom-left
	];

	const centerImageUrl = '/tby_logo_transparent_500x500.png'; // Replace with your large image URL

	function shuffle(arr) {
		let currentIndex = arr.length,
			randomIndex;

		while (currentIndex != 0) {
			randomIndex = Math.floor(Math.random() * currentIndex);
			currentIndex--;

			[arr[currentIndex], arr[randomIndex]] = [arr[randomIndex], arr[currentIndex]];
		}

		return arr;
	}

	function countWinningCombinations() {
		return winningPositions.filter((combination) => {
			return combination.every((index) => cells[index].strikeout);
		}).length;
	}

	function updateLetters() {
		let winningCount = countWinningCombinations();
		letters = letters.map((letter, i) =>
			i < winningCount ? letter.replace(' (show)', '') + ' (show)' : letter.replace(' (show)', '')
		);
	}

	function handleClick(index) {
		if (index !== 12) {
			// Prevent clicking on the center cell
			cells[index].strikeout = !cells[index].strikeout;
			updateLetters();

			if (countWinningCombinations() === 5) {
				showModal = true; // Show the modal
				triggerConfetti(); // Trigger confetti
			}
		}
	}
	function closeModal() {
		showModal = false;
		location.reload(); // Reload the page when the modal is closed
	}

	function triggerConfetti() {
		confetti({
			particleCount: 100,
			spread: 70,
			origin: { y: 0.6 }
		});
	}

	onMount(async () => {
		const response = await fetch('/phrases.json');
		phrases = await response.json();
		shuffle(phrases);
		cells = phrases.slice(0, 25).map((phrase) => ({ num: phrase, strikeout: false }));
		cells[12] = { num: 'Free', strikeout: true, isCenter: true }; // Set the center cell as "Free" and mark it as the center
		loading = false; // Set loading to false once phrases are populated
	});
</script>

<div class="flex flex-col items-center">
	{#if loading}
		<div class="flex flex-col justify-center items-center h-64">
			<span
				class="loading loading-bars loading-lg -rotate-45
			"
			></span>
			<p class="text-3xl max-md:text-2xl mt-10">Loading Custom Board...</p>
		</div>
	{:else}
		<table class="border-collapse p-4">
			{#each Array(5) as _, i}
				<tr>
					{#each Array(5) as _, j}
						<td
							class="border border-neutral-content max-md:w-20 max-md:h-20 w-28 h-28 text-center cursor-pointer max-md:text-sm p-2 max-md:p-0"
							on:click={() => handleClick(i * 5 + j)}
							class:strikeout={cells[i * 5 + j]?.strikeout}
						>
							{#if cells[i * 5 + j]?.isCenter}
								<img
									src={centerImageUrl}
									alt="Free Space with Basement Yard Logo"
									class="center-image bg-inherit"
								/>
							{:else}
								{cells[i * 5 + j]?.num}
							{/if}
						</td>
					{/each}
				</tr>
			{/each}
		</table>
	{/if}
</div>
<div class="mt-5 max-md:mb-2 mb-8">
	<table class="mx-auto">
		<tr>
			{#each letters as letter}
				<td class="font-anton text-5xl max-md:text-4xl" class:show-bingo={letter.includes('(show)')}
					>{letter.replace(' (show)', '')}</td
				>
			{/each}
		</tr>
	</table>
</div>
{#if showModal}
	<div class="modal modal-open">
		<div class="modal-box">
			<h2 class="font-bold text-2xl">BINGO!</h2>
			<p class="py-4">Thanks for Playing Basement Yard Bingo & Supporting the Basement Boys!</p>
			<div class="modal-action">
				<button class="btn" on:click={closeModal}>Close</button>
			</div>
		</div>
		<button class="modal-backdrop" on:click={closeModal}></button>
	</div>
{/if}

<style>
	.strikeout {
		text-decoration: line-through;
		color: #fc1e26;
	}
	.show-bingo {
		color: #fc1e26;
	}
</style>
