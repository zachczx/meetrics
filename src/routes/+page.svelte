<script lang="ts">
	let hours: number = $state();
	let minutes: number = $state();
</script>

<div class="grid min-h-dvh content-center justify-items-center gap-12">
	<header class="grid justify-items-center gap-4">
		<h1 class="flex text-4xl font-extrabold">
			<div>Me</div>
			<div class="-mt-2">e</div>
			<div>trics</div>
		</h1>
		<p>How much did your meeting cost?</p>
	</header>
	<main class="grid gap-8">
		<div class="grid grid-cols-2 gap-4">
			<div>
				<label
					><span class="label">Hours:</span>
					<select id="hour" bind:value={hours} class="select select-primary">
						<option value="1">1</option>
						<option value="2">2</option>
						<option value="3">3</option>
						<option value="4">4</option>
						<option value="5">5</option>
						<option value="6">6</option>
						<option value="7">7</option>
						<option value="8">8</option>
						<option value="9">9</option>
						<option value="10">10</option>
						<option value="11">11</option>
						<option value="12">12</option>
					</select></label
				>
			</div>
			<div>
				<label
					><span class="label">Minutes:</span>
					<select id="minute" bind:value={minutes} class="select select-primary">
						<option value="00">00</option>
						<option value="05">05</option>
						<option value="10">10</option>
						<option value="15">15</option>
						<option value="20">20</option>
						<option value="25">25</option>
						<option value="30">30</option>
						<option value="35">35</option>
						<option value="40">40</option>
						<option value="45">45</option>
						<option value="50">50</option>
						<option value="55">55</option>
					</select></label
				>
			</div>
		</div>
		<div class="grid grid-cols-4 gap-4">
			<label class="floating-label">
				<span class="label">Asst Mgr</span>
				<input type="text" placeholder="Assistant Managers" class="input input-primary" />
			</label>
			<label class="floating-label">
				<span class="label">Mgr</span>
				<input type="text" placeholder="Managers" class="input input-primary" />
			</label>

			<label class="floating-label">
				<span class="label">Snr Mgr</span>
				<input type="text" placeholder="Senior Managers" class="input input-primary" />
			</label>
		</div>
		<div class="grid grid-cols-4 gap-4">
			<label class="floating-label">
				<span class="label">Asst Dir</span>
				<input type="text" placeholder="Assistant Directors" class="input input-primary" />
			</label>

			<label class="floating-label">
				<span class="label">Snr Asst Dir</span>
				<input type="text" placeholder="Senior Assistant Directors" class="input input-primary" />
			</label>

			<label class="floating-label">
				<span class="label">Dy Dir</span>
				<input type="text" placeholder="Deputy Directors" class="input input-primary" />
			</label>

			<label class="floating-label">
				<span class="label">Snr Dy Dir</span>
				<input type="text" placeholder="Senior Deputy Directors" class="input input-primary" />
			</label>
		</div>
		<div class="grid grid-cols-4 gap-4">
			<label class="floating-label">
				<span class="label">Dir</span>
				<input type="text" placeholder="Directors" class="input input-primary" />
			</label>

			<label class="floating-label">
				<span class="label">Snr Dir</span>
				<input type="text" placeholder="Senior Directors" class="input input-primary" />
			</label>
		</div>

		<div class="grid grid-cols-4 gap-4">
			<label class="floating-label">
				<span class="label">DS</span>
				<input type="text" placeholder="Deputy Secretaries" class="input input-primary" />
			</label>

			<label class="floating-label">
				<span class="label">PS</span>
				<input type="text" placeholder="Permanant Secretaries" class="input input-primary" />
			</label>
		</div>
	</main>
</div>
