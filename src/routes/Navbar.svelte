<div class="navbar bg-base-100">
	<div class="navbar-start">
		<!-- Dropdown Menu for Mobile View -->
		<div class="dropdown">
			<div tabindex="0" role="button" class="btn btn-ghost lg:hidden">
				<svg
					xmlns="http://www.w3.org/2000/svg"
					class="h-5 w-5"
					fill="none"
					viewBox="0 0 24 24"
					stroke="currentColor"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M4 6h16M4 12h8m-8 6h16"
					/>
				</svg>
			</div>
			<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
			<ul
				tabindex="0"
				class="menu menu-sm dropdown-content bg-base-100 rounded-box z-[1] mt-3 w-52 p-2 shadow"
			>
				<li><a href="/">Play</a></li>
				<li><a href="/about">About</a></li>
				<li><a href="/contact">Contact</a></li>
			</ul>
		</div>
		<a href="/" class="btn btn-ghost font-anton font-extralight text-xl"
			>The Basement Yard Bingo <span
				><img
					src="/favicon-32x32.png"
					class="h-[24px] w-[24px]"
					alt="basement yard bing logo"
				/></span
			></a
		>
	</div>
	<div class="navbar-center hidden lg:flex">
		<ul class="menu menu-horizontal px-1">
			<li><a href="/">Play</a></li>
			<li><a href="/about">About</a></li>
			<li><a href="/contact">Contact</a></li>
		</ul>
	</div>
	<div class="navbar-end">
		<!-- <a href="/" class="btn">Button</a> -->
	</div>
</div>
