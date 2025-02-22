<script lang="ts">
	let hours: string = $state('1');
	let minutes: string = $state('00');

	interface Count {
		asstMgr: number;
		mgr: number;
		snrMgr: number;
		asstDir: number;
		snrAsstDir: number;
		dyDir: number;
		dir: number;
	}

	interface SalaryRange {
		asstMgr: { min: number; max: number };
		mgr: { min: number; max: number };
		snrMgr: { min: number; max: number };
		asstDir: { min: number; max: number };
		snrAsstDir: { min: number; max: number };
		dyDir: { min: number; max: number };
		dir: { min: number; max: number };
	}

	let count: Count = $state({
		asstMgr: 0,
		mgr: 0,
		snrMgr: 0,
		asstDir: 0,
		snrAsstDir: 0,
		dyDir: 0,
		dir: 0
	});

	let toggleMinMax: 'min' | 'max' | 'avg' = $state('min');

	// https://blog.seedly.sg/civil-service-salary/
	const salariesMonthly: SalaryRange = {
		asstMgr: { min: 3300, max: 6250 },
		mgr: { min: 3300, max: 6250 },
		snrMgr: { min: 3300, max: 6250 },
		asstDir: { min: 4800, max: 7700 },
		snrAsstDir: { min: 5900, max: 9700 },
		dyDir: { min: 7600, max: 12300 },
		dir: { min: 16700, max: 22700 }
	};

	let meetingCost = $derived.by(() => {
		let meetingCost = 0;
		let totalTimeInMins = Number(hours) * 60 + Number(minutes);

		for (let rank in count) {
			if (toggleMinMax === 'min') {
				meetingCost +=
					getSalariesPerMinute(
						count[rank as keyof Count],
						salariesMonthly[rank as keyof SalaryRange].min
					) * totalTimeInMins;
			}
			if (toggleMinMax === 'max') {
				meetingCost +=
					getSalariesPerMinute(
						count[rank as keyof Count],
						salariesMonthly[rank as keyof SalaryRange].max
					) * totalTimeInMins;
			}
		}
		return meetingCost.toFixed(2);
	});

	function getSalariesPerMinute(personCount: number, salaryMonthly: number) {
		const numberMinutesInWorkDay = 570; // Assumes 9am to 6.30pm
		const salaryMinutely = (personCount * salaryMonthly) / (28 * numberMinutesInWorkDay);
		return salaryMinutely;
	}
</script>

<div class="grid min-h-dvh content-center justify-items-center gap-16">
	<header class="grid justify-items-center gap-4">
		<h1 class="group flex text-7xl font-extrabold transition-all duration-1000 ease-out">
			<div class="text-secondary">Me</div>
			<div class="text-secondary group-hover:-translate-y-4">e</div>
			<div class="text-secondary">trics</div>
		</h1>
		<p>How much did your meeting cost?</p>
	</header>
	<main class="grid gap-12">
		<div class="grid grid-cols-7 items-center gap-4">
			<div>
				<svg
					xmlns="http://www.w3.org/2000/svg"
					width="32"
					height="32"
					class="material-symbols:timer inline-block h-20 w-20"
					viewBox="0 0 24 24"
					><path
						fill="currentColor"
						d="M9 3V1h6v2zm2 11h2V8h-2zm1 8q-1.85 0-3.488-.712T5.65 19.35t-1.937-2.863T3 13t.713-3.488T5.65 6.65t2.863-1.937T12 4q1.55 0 2.975.5t2.675 1.45l1.4-1.4l1.4 1.4l-1.4 1.4Q20 8.6 20.5 10.025T21 13q0 1.85-.713 3.488T18.35 19.35t-2.863 1.938T12 22"
					/></svg
				>
			</div>
			<div class="col-span-6 grid grid-cols-3 gap-4">
				<div>
					<label
						><span class="label">Hours:</span>
						<select id="hour" bind:value={hours} class="select">
							<option value="0">0</option>
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
						</select>
					</label>
				</div>
				<div>
					<label
						><span class="label">Minutes:</span>
						<select id="minute" bind:value={minutes} class="select">
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
						</select>
					</label>
				</div>
			</div>
		</div>
		<div class="grid grid-cols-7 items-center gap-4">
			<div>
				<svg
					xmlns="http://www.w3.org/2000/svg"
					width="32"
					height="32"
					class="clarity:users-solid h-20 w-20"
					viewBox="0 0 36 36"
					><path
						fill="currentColor"
						d="M12 16.14h-.87a8.67 8.67 0 0 0-6.43 2.52l-.24.28v8.28h4.08v-4.7l.55-.62l.25-.29a11 11 0 0 1 4.71-2.86A6.6 6.6 0 0 1 12 16.14"
						class="clr-i-solid clr-i-solid-path-1"
					/><path
						fill="currentColor"
						d="M31.34 18.63a8.67 8.67 0 0 0-6.43-2.52a11 11 0 0 0-1.09.06a6.6 6.6 0 0 1-2 2.45a10.9 10.9 0 0 1 5 3l.25.28l.54.62v4.71h3.94v-8.32Z"
						class="clr-i-solid clr-i-solid-path-2"
					/><path
						fill="currentColor"
						d="M11.1 14.19h.31a6.45 6.45 0 0 1 3.11-6.29a4.09 4.09 0 1 0-3.42 6.33Z"
						class="clr-i-solid clr-i-solid-path-3"
					/><path
						fill="currentColor"
						d="M24.43 13.44a7 7 0 0 1 0 .69a4 4 0 0 0 .58.05h.19A4.09 4.09 0 1 0 21.47 8a6.53 6.53 0 0 1 2.96 5.44"
						class="clr-i-solid clr-i-solid-path-4"
					/><circle
						cx="17.87"
						cy="13.45"
						r="4.47"
						fill="currentColor"
						class="clr-i-solid clr-i-solid-path-5"
					/><path
						fill="currentColor"
						d="M18.11 20.3A9.7 9.7 0 0 0 11 23l-.25.28v6.33a1.57 1.57 0 0 0 1.6 1.54h11.49a1.57 1.57 0 0 0 1.6-1.54V23.3l-.24-.3a9.58 9.58 0 0 0-7.09-2.7"
						class="clr-i-solid clr-i-solid-path-6"
					/><path fill="none" d="M0 0h36v36H0z" /></svg
				>
			</div>

			<div class="col-span-6 grid grid-cols-3 gap-4">
				<label class="floating-label">
					<span class="label">Asst Mgr</span>
					<input
						type="text"
						bind:value={count.asstMgr}
						placeholder="Assistant Managers"
						class="input"
					/>
				</label>
				<label class="floating-label">
					<span class="label">Mgr</span>
					<input type="text" bind:value={count.mgr} placeholder="Managers" class="input" />
				</label>

				<label class="floating-label">
					<span class="label">Snr Mgr</span>
					<input
						type="text"
						bind:value={count.snrMgr}
						placeholder="Senior Managers"
						class="input"
					/>
				</label>

				<label class="floating-label">
					<span class="label">Asst Dir</span>
					<input
						type="text"
						bind:value={count.asstDir}
						placeholder="Assistant Directors"
						class="input"
					/>
				</label>

				<label class="floating-label">
					<span class="label">Snr Asst Dir</span>
					<input
						type="text"
						bind:value={count.snrAsstDir}
						placeholder="Senior Assistant Directors"
						class="input"
					/>
				</label>

				<label class="floating-label">
					<span class="label">Dy Dir</span>
					<input
						type="text"
						bind:value={count.dyDir}
						placeholder="Deputy Directors"
						class="input"
					/>
				</label>

				<label class="floating-label">
					<span class="label">Dir</span>
					<input type="text" bind:value={count.dir} placeholder="Directors" class="input" />
				</label>
			</div>
		</div>

		<div class="grid grid-cols-7 items-center gap-4">
			<div>
				<svg
					xmlns="http://www.w3.org/2000/svg"
					width="32"
					height="32"
					class="material-symbols:attach-money h-20 w-20"
					viewBox="0 0 24 24"
					><path
						fill="currentColor"
						d="M11.025 21v-2.15q-1.325-.3-2.287-1.15t-1.413-2.4l1.85-.75q.375 1.2 1.113 1.825t1.937.625q1.025 0 1.738-.462t.712-1.438q0-.875-.55-1.387t-2.55-1.163q-2.15-.675-2.95-1.612t-.8-2.288q0-1.625 1.05-2.525t2.15-1.025V3h2v2.1q1.25.2 2.063.913t1.187 1.737l-1.85.8q-.3-.8-.85-1.2t-1.5-.4q-1.1 0-1.675.488T9.825 8.65q0 .825.75 1.3t2.6 1q1.725.5 2.613 1.588t.887 2.512q0 1.775-1.05 2.7t-2.6 1.15V21z"
					/></svg
				>
			</div>
			<div class="col-span-6 grid grid-cols-3 gap-4">
				<label
					><span class="label">Salary range</span>
					<select class="select" bind:value={toggleMinMax}>
						<option value="min">Minimum</option>
						<option value="max">Maximum</option>
					</select>
				</label>
			</div>
		</div>

		<h1 class="text-xl">
			Your meeting costs at least <span class="text-5xl font-extrabold">${meetingCost}</span>
		</h1>
	</main>
</div>
