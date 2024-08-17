<script>
	import { onMount } from 'svelte';

	let phrases = [];
	let cells = [];
	let letters = ['B', 'I', 'N', 'G', 'O'];
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
		[4, 9, 14, 19, 24]
	];

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
		cells[index].strikeout = !cells[index].strikeout;
		updateLetters();

		if (countWinningCombinations() === 5) {
			alert('B I N G O');
			location.reload();
		}
	}

	onMount(async () => {
		const response = await fetch('/phrases.json');
		phrases = await response.json();
		shuffle(phrases);
		cells = phrases.slice(0, 25).map((phrase) => ({ num: phrase, strikeout: false }));
		cells[12] = { num: 'Free', strikeout: true }; // Set the center cell as "Free"
	});
</script>

<div class="flex flex-col items-center">
	<table class="border-collapse">
		{#each Array(5) as _, i}
			<tr>
				{#each Array(5) as _, j}
					<td
						class="border border-neutral-content w-20 h-20 text-center cursor-pointer"
						on:click={() => handleClick(i * 5 + j)}
						class:strikeout={cells[i * 5 + j]?.strikeout}
					>
						{cells[i * 5 + j]?.num}
					</td>
				{/each}
			</tr>
		{/each}
	</table>
</div>
<div class="mt-5">
	<table class="mx-auto">
		<tr>
			{#each letters as letter}
				<td class="font-anton text-3xl mx-2" class:show-bingo={letter.includes('(show)')}
					>{letter.replace(' (show)', '')}</td
				>
			{/each}
		</tr>
	</table>
</div>

<style>
	.strikeout {
		text-decoration: line-through;
	}
	.show-bingo {
		color: #fc1e26;
	}
</style>
