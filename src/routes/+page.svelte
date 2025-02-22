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
		asstMgr: 1,
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

	let salaryRangeMoreText: boolean = $state(false);
</script>

<div class="grid min-h-dvh content-center justify-items-center gap-16">
	<header class="grid justify-items-center gap-4">
		<a href="/"
			><h1 class="group flex text-7xl font-extrabold">
				<div class="text-secondary">Me</div>
				<div
					class="text-secondary group-hover:text-info transition-all duration-300 ease-out group-hover:-translate-y-4"
				>
					e
				</div>
				<div class="text-secondary">trics</div>
			</h1></a
		>

		<p>How much did your meeting cost?</p>
	</header>
	<main class="grid gap-12">
		<div class="grid grid-cols-7 items-center gap-8">
			<div>
				<!-- <svg
					xmlns="http://www.w3.org/2000/svg"
					width="32"
					height="32"
					class="material-symbols:timer inline-block h-20 w-20"
					viewBox="0 0 24 24"
					><path
						fill="currentColor"
						d="M9 3V1h6v2zm2 11h2V8h-2zm1 8q-1.85 0-3.488-.712T5.65 19.35t-1.937-2.863T3 13t.713-3.488T5.65 6.65t2.863-1.937T12 4q1.55 0 2.975.5t2.675 1.45l1.4-1.4l1.4 1.4l-1.4 1.4Q20 8.6 20.5 10.025T21 13q0 1.85-.713 3.488T18.35 19.35t-2.863 1.938T12 22"
					/></svg
				> -->
				<svg
					xmlns="http://www.w3.org/2000/svg"
					width="32"
					height="32"
					class="fluent-color:clock-alarm-16 inline-block h-20 w-20"
					viewBox="0 0 16 16"
					><g fill="none"
						><path
							fill="url(#fluentColorClockAlarm160)"
							d="M12.888 6.58A2.5 2.5 0 1 0 9.254 3.4a5.26 5.26 0 0 1 3.634 3.18"
						/><path
							fill="url(#fluentColorClockAlarm167)"
							d="M12.888 6.58A2.5 2.5 0 1 0 9.254 3.4a5.26 5.26 0 0 1 3.634 3.18"
						/><path
							fill="url(#fluentColorClockAlarm161)"
							d="M6.746 3.4a5.26 5.26 0 0 0-3.634 3.18A2.5 2.5 0 1 1 6.746 3.4"
						/><path
							fill="url(#fluentColorClockAlarm168)"
							d="M6.746 3.4a5.26 5.26 0 0 0-3.634 3.18A2.5 2.5 0 1 1 6.746 3.4"
						/><path
							fill="url(#fluentColorClockAlarm162)"
							d="m3.354 13.854l1.5-1.5l-.708-.707l-1.5 1.5a.5.5 0 1 0 .708.707"
						/><path
							fill="url(#fluentColorClockAlarm163)"
							d="m13.354 13.147l-1.5-1.5l-.708.707l1.5 1.5a.5.5 0 1 0 .707-.707"
						/><path
							fill="url(#fluentColorClockAlarm164)"
							d="M13.5 8.5a5.5 5.5 0 1 1-11 0a5.5 5.5 0 0 1 11 0"
						/><path
							fill="url(#fluentColorClockAlarm165)"
							d="M12.5 8.5a4.5 4.5 0 1 1-9 0a4.5 4.5 0 0 1 9 0"
						/><path
							fill="url(#fluentColorClockAlarm166)"
							fill-rule="evenodd"
							d="M7.5 5.5A.5.5 0 0 1 8 6v2.5h1.5a.5.5 0 0 1 0 1h-2A.5.5 0 0 1 7 9V6a.5.5 0 0 1 .5-.5"
							clip-rule="evenodd"
						/><defs
							><linearGradient
								id="fluentColorClockAlarm160"
								x1="14.807"
								x2="10.139"
								y1="6.913"
								y2="3.094"
								gradientUnits="userSpaceOnUse"
								><stop stop-color="#FF6F47" /><stop
									offset="1"
									stop-color="#FFCD0F"
								/></linearGradient
							><linearGradient
								id="fluentColorClockAlarm161"
								x1="6.922"
								x2="2.254"
								y1="6.913"
								y2="3.094"
								gradientUnits="userSpaceOnUse"
								><stop stop-color="#FF6F47" /><stop
									offset="1"
									stop-color="#FFCD0F"
								/></linearGradient
							><linearGradient
								id="fluentColorClockAlarm162"
								x1="2.5"
								x2="2.973"
								y1="10.636"
								y2="14.775"
								gradientUnits="userSpaceOnUse"
								><stop stop-color="#CAD2D9" /><stop
									offset="1"
									stop-color="#70777D"
								/></linearGradient
							><linearGradient
								id="fluentColorClockAlarm163"
								x1="2.5"
								x2="2.973"
								y1="10.636"
								y2="14.775"
								gradientUnits="userSpaceOnUse"
								><stop stop-color="#CAD2D9" /><stop
									offset="1"
									stop-color="#70777D"
								/></linearGradient
							><linearGradient
								id="fluentColorClockAlarm164"
								x1="4.333"
								x2="9.833"
								y1="2.389"
								y2="14.611"
								gradientUnits="userSpaceOnUse"
								><stop stop-color="#1EC8B0" /><stop
									offset="1"
									stop-color="#2764E7"
								/></linearGradient
							><linearGradient
								id="fluentColorClockAlarm165"
								x1="4.87"
								x2="10.739"
								y1="3.022"
								y2="15.543"
								gradientUnits="userSpaceOnUse"
								><stop stop-color="#FDFDFD" /><stop
									offset="1"
									stop-color="#DEDEFF"
								/></linearGradient
							><linearGradient
								id="fluentColorClockAlarm166"
								x1="8.4"
								x2="10.286"
								y1="5.422"
								y2="8.566"
								gradientUnits="userSpaceOnUse"
								><stop stop-color="#1EC8B0" /><stop
									offset="1"
									stop-color="#2764E7"
								/></linearGradient
							><radialGradient
								id="fluentColorClockAlarm167"
								cx="0"
								cy="0"
								r="1"
								gradientTransform="matrix(5.2 -5.2 5.2 5.2 8.4 8.4)"
								gradientUnits="userSpaceOnUse"
								><stop offset=".644" stop-color="#FF6F47" /><stop
									offset=".942"
									stop-color="#FF6F47"
									stop-opacity="0"
								/></radialGradient
							><radialGradient
								id="fluentColorClockAlarm168"
								cx="0"
								cy="0"
								r="1"
								gradientTransform="matrix(-5.2 -5.2 5.2 -5.2 8.4 8.4)"
								gradientUnits="userSpaceOnUse"
								><stop offset=".659" stop-color="#FF6F47" /><stop
									offset=".949"
									stop-color="#FF6F47"
									stop-opacity="0"
								/></radialGradient
							></defs
						></g
					></svg
				>
			</div>
			<div class="col-span-6 grid grid-cols-3 gap-4">
				<div>
					<label class="floating-label"
						><span class="label {hours !== '0' ? 'text-secondary' : undefined}">Hours:</span>
						<select
							id="hour"
							bind:value={hours}
							class="select {hours !== '0' ? 'select-secondary text-secondary' : undefined}"
						>
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
					<label class="floating-label"
						><span class="label {minutes !== '00' ? 'text-secondary' : undefined}">Minutes:</span>
						<select
							id="minute"
							bind:value={minutes}
							class="select {minutes !== '00' ? 'select-secondary text-secondary' : undefined}"
						>
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
		<div class="grid grid-cols-7 items-center gap-8">
			<div>
				<!-- <svg
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
				> -->
				<svg
					xmlns="http://www.w3.org/2000/svg"
					width="32"
					height="32"
					class="fluent-color:people-community-16 h-20 w-20"
					viewBox="0 0 16 16"
					><g fill="none"
						><path
							fill="url(#fluentColorPeopleCommunity160)"
							d="M10.99 7.714a1.5 1.5 0 0 0-1.838 1.061l-.388 1.449a3 3 0 1 0 5.796 1.553l.388-1.45a1.5 1.5 0 0 0-1.06-1.836z"
						/><path
							fill="url(#fluentColorPeopleCommunity161)"
							d="M5.01 7.714a1.5 1.5 0 0 1 1.837 1.061l.388 1.449a3 3 0 1 1-5.795 1.553l-.389-1.45a1.5 1.5 0 0 1 1.061-1.836z"
						/><path
							fill="url(#fluentColorPeopleCommunity162)"
							d="M6.5 7A1.5 1.5 0 0 0 5 8.5V11a3 3 0 1 0 6 0V8.5A1.5 1.5 0 0 0 9.5 7z"
						/><path
							fill="url(#fluentColorPeopleCommunity163)"
							d="M8 1a2.5 2.5 0 1 0 0 5a2.5 2.5 0 0 0 0-5"
						/><path
							fill="url(#fluentColorPeopleCommunity164)"
							d="M3 3a2 2 0 1 0 0 4a2 2 0 0 0 0-4"
						/><path
							fill="url(#fluentColorPeopleCommunity165)"
							d="M13 3a2 2 0 1 0 0 4a2 2 0 0 0 0-4"
						/><defs
							><radialGradient
								id="fluentColorPeopleCommunity160"
								cx="0"
								cy="0"
								r="1"
								gradientTransform="rotate(78.837 -.336 11.297)scale(4.64914)"
								gradientUnits="userSpaceOnUse"
								><stop stop-color="#0078D4" /><stop
									offset="1"
									stop-color="#004695"
								/></radialGradient
							><radialGradient
								id="fluentColorPeopleCommunity161"
								cx="0"
								cy="0"
								r="1"
								gradientTransform="matrix(3.34115 6.04144 -4.34865 2.40497 2.553 7.96)"
								gradientUnits="userSpaceOnUse"
								><stop stop-color="#008CE2" /><stop
									offset="1"
									stop-color="#0068C6"
								/></radialGradient
							><radialGradient
								id="fluentColorPeopleCommunity162"
								cx="0"
								cy="0"
								r="1"
								gradientTransform="rotate(63.608 -3.915 10.713)scale(4.22417 3.87907)"
								gradientUnits="userSpaceOnUse"
								><stop offset=".339" stop-color="#3DCBFF" /><stop
									offset="1"
									stop-color="#14B1FF"
								/></radialGradient
							><radialGradient
								id="fluentColorPeopleCommunity163"
								cx="0"
								cy="0"
								r="1"
								gradientTransform="rotate(59.931 1.37 7.898)scale(3.12306)"
								gradientUnits="userSpaceOnUse"
								><stop offset=".339" stop-color="#3DCBFF" /><stop
									offset="1"
									stop-color="#14B1FF"
								/></radialGradient
							><radialGradient
								id="fluentColorPeopleCommunity164"
								cx="0"
								cy="0"
								r="1"
								gradientTransform="rotate(47.573 -3.7 4.554)scale(3.27979)"
								gradientUnits="userSpaceOnUse"
								><stop stop-color="#008CE2" /><stop
									offset="1"
									stop-color="#0068C6"
								/></radialGradient
							><radialGradient
								id="fluentColorPeopleCommunity165"
								cx="0"
								cy="0"
								r="1"
								gradientTransform="rotate(78.837 3.672 9.578)scale(2.93403)"
								gradientUnits="userSpaceOnUse"
								><stop stop-color="#0078D4" /><stop
									offset="1"
									stop-color="#004695"
								/></radialGradient
							></defs
						></g
					></svg
				>
			</div>

			<div class="col-span-6 grid grid-cols-3 gap-4">
				<label class="floating-label">
					<span class="label {count.asstMgr > 0 ? 'text-secondary' : undefined}">Asst Mgr</span>
					<input
						type="text"
						bind:value={count.asstMgr}
						placeholder="Assistant Managers"
						class="input {count.asstMgr > 0 ? 'input-secondary text-secondary' : undefined}"
					/>
				</label>
				<label class="floating-label">
					<span class="label {count.mgr > 0 ? 'text-secondary' : undefined}">Mgr</span>
					<input
						type="text"
						bind:value={count.mgr}
						placeholder="Managers"
						class="input {count.mgr > 0 ? 'input-secondary text-secondary' : undefined}"
					/>
				</label>

				<label class="floating-label">
					<span class="label {count.snrMgr > 0 ? 'text-secondary' : undefined}">Snr Mgr</span>
					<input
						type="text"
						bind:value={count.snrMgr}
						placeholder="Senior Managers"
						class="input {count.snrMgr > 0 ? 'input-secondary text-secondary' : undefined}"
					/>
				</label>

				<label class="floating-label">
					<span class="label {count.asstDir > 0 ? 'text-secondary' : undefined}">Asst Dir</span>
					<input
						type="text"
						bind:value={count.asstDir}
						placeholder="Assistant Directors"
						class="input {count.asstDir > 0 ? 'input-secondary text-secondary' : undefined}"
					/>
				</label>

				<label class="floating-label">
					<span class="label {count.snrAsstDir > 0 ? 'text-secondary' : undefined}"
						>Snr Asst Dir</span
					>
					<input
						type="text"
						bind:value={count.snrAsstDir}
						placeholder="Senior Assistant Directors"
						class="input {count.snrAsstDir > 0 ? 'input-secondary text-secondary' : undefined}"
					/>
				</label>

				<label class="floating-label">
					<span class="label {count.dyDir > 0 ? 'text-secondary' : undefined}">Dy Dir</span>
					<input
						type="text"
						bind:value={count.dyDir}
						placeholder="Deputy Directors"
						class="input {count.dyDir > 0 ? 'input-secondary text-secondary' : undefined}"
					/>
				</label>

				<label class="floating-label">
					<span class="label {count.dir > 0 ? 'text-secondary' : undefined}">Dir</span>
					<input
						type="text"
						bind:value={count.dir}
						placeholder="Directors"
						class="input {count.dir > 0 ? 'input-secondary text-secondary' : undefined}"
					/>
				</label>
			</div>
		</div>

		<div class="grid grid-cols-7 items-center gap-8">
			<div>
				<!-- <svg
					xmlns="http://www.w3.org/2000/svg"
					width="32"
					height="32"
					class="material-symbols:attach-money h-20 w-20"
					viewBox="0 0 24 24"
					><path
						fill="currentColor"
						d="M11.025 21v-2.15q-1.325-.3-2.287-1.15t-1.413-2.4l1.85-.75q.375 1.2 1.113 1.825t1.937.625q1.025 0 1.738-.462t.712-1.438q0-.875-.55-1.387t-2.55-1.163q-2.15-.675-2.95-1.612t-.8-2.288q0-1.625 1.05-2.525t2.15-1.025V3h2v2.1q1.25.2 2.063.913t1.187 1.737l-1.85.8q-.3-.8-.85-1.2t-1.5-.4q-1.1 0-1.675.488T9.825 8.65q0 .825.75 1.3t2.6 1q1.725.5 2.613 1.588t.887 2.512q0 1.775-1.05 2.7t-2.6 1.15V21z"
					/></svg
				> -->
				<svg
					xmlns="http://www.w3.org/2000/svg"
					width="32"
					height="32"
					class="fluent-color:savings-16 h-20 w-20"
					viewBox="0 0 16 16"
					><g fill="none"
						><path
							fill="url(#fluentColorSavings160)"
							d="M15 8.268c0 1.552-.548 2.64-1.11 3.34a4.6 4.6 0 0 1-.89.85V13.5a1.5 1.5 0 0 1-1.5 1.5H11a1 1 0 0 1-1-1H8a1 1 0 0 1-1 1h-.5A1.5 1.5 0 0 1 5 13.5v-.214c-1.363-.433-2.312-1.534-2.78-2.858a.43.43 0 0 0-.269-.281A1.31 1.31 0 0 1 1 8.89v-.794c0-.576.382-1.082.935-1.24c.104-.03.216-.13.271-.297c.346-1.046 1.069-1.81 2-2.37q.145-.088.26-.15V2.163a.76.76 0 0 1 .481-.704a.77.77 0 0 1 .826.15c.253.24.585.525.94.758c.365.238.707.39.986.418h.005C11.077 2.784 15 4.471 15 8.268"
						/><path
							fill="url(#fluentColorSavings161)"
							d="M5.5 6.75a.75.75 0 1 1-1.5 0a.75.75 0 0 1 1.5 0"
						/><path
							fill="#9F1459"
							d="M6.942 4.727a.835.835 0 0 1 1.09-.453l5.56 2.295a.835.835 0 0 1-.637 1.544l-5.56-2.296a.835.835 0 0 1-.453-1.09"
						/><path
							fill="url(#fluentColorSavings162)"
							d="M12.14 7.776a3 3 0 1 0-3.906-1.613z"
						/><path
							fill="url(#fluentColorSavings163)"
							fill-opacity=".8"
							d="M12.14 7.776a3 3 0 1 0-3.906-1.613z"
						/><defs
							><radialGradient
								id="fluentColorSavings160"
								cx="0"
								cy="0"
								r="1"
								gradientTransform="matrix(3.63124 12.79178 -13.17677 3.74053 5.287 2.736)"
								gradientUnits="userSpaceOnUse"
								><stop stop-color="#F08AF4" /><stop offset=".581" stop-color="#E869CE" /><stop
									offset="1"
									stop-color="#D7257D"
								/></radialGradient
							><radialGradient
								id="fluentColorSavings161"
								cx="0"
								cy="0"
								r="1"
								gradientTransform="rotate(59.532 -3.375 7.091)scale(1.27093)"
								gradientUnits="userSpaceOnUse"
								><stop stop-color="#B91D6B" /><stop
									offset="1"
									stop-color="#670938"
								/></radialGradient
							><linearGradient
								id="fluentColorSavings162"
								x1="12.968"
								x2="8.03"
								y1="7.63"
								y2="3.794"
								gradientUnits="userSpaceOnUse"
								><stop stop-color="#FF8A69" /><stop
									offset="1"
									stop-color="#FFCD0F"
								/></linearGradient
							><linearGradient
								id="fluentColorSavings163"
								x1="11.795"
								x2="9.488"
								y1="2.361"
								y2="7.421"
								gradientUnits="userSpaceOnUse"
								><stop offset=".67" stop-color="#FB5937" stop-opacity="0" /><stop
									offset="1"
									stop-color="#CD3E1D"
								/></linearGradient
							></defs
						></g
					></svg
				>
			</div>
			<div class="col-span-6 grid grid-cols-3 content-center gap-4">
				<label class="floating-label"
					><span class="label text-secondary">Salary range</span>
					<select class="select select-secondary text-secondary" bind:value={toggleMinMax}>
						<option value="min">Minimum</option>
						<option value="max">Maximum</option>
					</select>
				</label>

				<div
					class="tooltip tooltip-primary self-center"
					data-tip="Choose either the minimum or maximum figure in salary ranges"
				>
					<button class="flex items-center text-xl" aria-label="What's this?"
						><svg
							xmlns="http://www.w3.org/2000/svg"
							width="1em"
							height="1em"
							class="material-symbols:help-outline"
							viewBox="0 0 24 24"
							><path
								fill="currentColor"
								d="M11.95 18q.525 0 .888-.363t.362-.887t-.362-.888t-.888-.362t-.887.363t-.363.887t.363.888t.887.362m-.9-3.85h1.85q0-.825.188-1.3t1.062-1.3q.65-.65 1.025-1.238T15.55 8.9q0-1.4-1.025-2.15T12.1 6q-1.425 0-2.312.75T8.55 8.55l1.65.65q.125-.45.563-.975T12.1 7.7q.8 0 1.2.438t.4.962q0 .5-.3.938t-.75.812q-1.1.975-1.35 1.475t-.25 1.825M12 22q-2.075 0-3.9-.787t-3.175-2.138T2.788 15.9T2 12t.788-3.9t2.137-3.175T8.1 2.788T12 2t3.9.788t3.175 2.137T21.213 8.1T22 12t-.788 3.9t-2.137 3.175t-3.175 2.138T12 22m0-2q3.35 0 5.675-2.325T20 12t-2.325-5.675T12 4T6.325 6.325T4 12t2.325 5.675T12 20m0-8"
							/></svg
						></button
					>
				</div>
			</div>
		</div>

		<h1 class="text-xl">
			Your meeting costs <span class="text-secondary text-5xl font-extrabold">${meetingCost}</span>
		</h1>
	</main>
</div>
