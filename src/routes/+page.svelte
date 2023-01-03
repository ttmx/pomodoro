<script>
	const studyTime = 1 * 5;
	const restTime = 1 * 5;
	const tps = 60;

	let settingsOpen = false;
	let timer = 0;

	$: displayTime = Math.floor(
		timer < studyTime ? studyTime - timer : restTime - (timer - studyTime)
	);

	let timerRunning = false;

	setInterval(() => {
		if (timerRunning) {
			timer = (timer + 1 / tps) % (studyTime + restTime);
		}
	}, (1 / tps) * 1000);
</script>

<div
	class="w-full h-screen flex items-center justify-center bg-gradient-to-tr from-blue-300 to-purple-300"
>
	<div
		class="relative flex flex-col w-2/3 h-2/3 rounded-3xl bg-white bg-opacity-20 backdrop-blur-md items-center border-[1px] border-gray-400 border-opacity-50"
	>
		<div
			on:click={() => (settingsOpen = !settingsOpen)}
			class="absolute top-0 right-0 fill-slate-900 w-12 m-4 cursor-pointer hover:scale-105 transition hover:shadow-lg p-2 rounded-xl hover:bg-slate-200 hover:bg-opacity-30 border-[1px] border-transparent hover:border-slate-500 hover:border-opacity-20"
		>
			<svg xmlns="http://www.w3.org/2000/svg" class="w-full h-full" viewBox="0 0 512 512">
				<!--! Font Awesome Pro 6.2.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. -->
				<path
					d="M495.9 166.6c3.2 8.7 .5 18.4-6.4 24.6l-43.3 39.4c1.1 8.3 1.7 16.8 1.7 25.4s-.6 17.1-1.7 25.4l43.3 39.4c6.9 6.2 9.6 15.9 6.4 24.6c-4.4 11.9-9.7 23.3-15.8 34.3l-4.7 8.1c-6.6 11-14 21.4-22.1 31.2c-5.9 7.2-15.7 9.6-24.5 6.8l-55.7-17.7c-13.4 10.3-28.2 18.9-44 25.4l-12.5 57.1c-2 9.1-9 16.3-18.2 17.8c-13.8 2.3-28 3.5-42.5 3.5s-28.7-1.2-42.5-3.5c-9.2-1.5-16.2-8.7-18.2-17.8l-12.5-57.1c-15.8-6.5-30.6-15.1-44-25.4L83.1 425.9c-8.8 2.8-18.6 .3-24.5-6.8c-8.1-9.8-15.5-20.2-22.1-31.2l-4.7-8.1c-6.1-11-11.4-22.4-15.8-34.3c-3.2-8.7-.5-18.4 6.4-24.6l43.3-39.4C64.6 273.1 64 264.6 64 256s.6-17.1 1.7-25.4L22.4 191.2c-6.9-6.2-9.6-15.9-6.4-24.6c4.4-11.9 9.7-23.3 15.8-34.3l4.7-8.1c6.6-11 14-21.4 22.1-31.2c5.9-7.2 15.7-9.6 24.5-6.8l55.7 17.7c13.4-10.3 28.2-18.9 44-25.4l12.5-57.1c2-9.1 9-16.3 18.2-17.8C227.3 1.2 241.5 0 256 0s28.7 1.2 42.5 3.5c9.2 1.5 16.2 8.7 18.2 17.8l12.5 57.1c15.8 6.5 30.6 15.1 44 25.4l55.7-17.7c8.8-2.8 18.6-.3 24.5 6.8c8.1 9.8 15.5 20.2 22.1 31.2l4.7 8.1c6.1 11 11.4 22.4 15.8 34.3zM256 336c44.2 0 80-35.8 80-80s-35.8-80-80-80s-80 35.8-80 80s35.8 80 80 80z"
				/>
			</svg>
		</div>
		<div class="text-center font-black text-[20rem] text-slate-900 leading-none pt-32 font-mono">
			{('' + Math.floor(displayTime / 60)).padStart(2, '0')}:{('' + (displayTime % 60)).padStart(
				2,
				'0'
			)}
		</div>
		<div class="w-4/5 h-4 flex flex-row rounded-full">
			<div
				style:flex-grow={studyTime}
				class="bg-blue-200 rounded-l-full border-[1px] border-blue-300 -mr-2"
			>
				<div
					style:width={Math.min(100, (timer / studyTime) * 100) + '%'}
					class="bg-blue-400 -m-px p-[7px] h-full rounded-full border-[1px] border-blue-500"
				/>
			</div>
			<div
				style:flex-grow={restTime}
				class="bg-emerald-200 rounded-r-full border-[1px] border-emerald-300"
			>
				<div
					style:width={Math.max(0, ((timer - studyTime) / restTime) * 100) + '%'}
					class:opacity-0={timer <= studyTime}
					class="bg-emerald-400 -m-px py-[7px] h-full rounded-full rounded-l-none border-[1px] border-emerald-500"
				/>
			</div>
		</div>
		<div class="flex flex-row w-full px-16 pt-8 items-center">
			<div class="flex flex-row grow w-full justify-around">
				<div
					on:click={() => (timerRunning = true)}
					class="font-bold text-6xl bg-emerald-400 rounded-3xl p-4 w-56 text-slate-100 text-center border-[1px] border-emerald-600 hover:bg-emerald-600 transition cursor-pointer"
				>
					Start
				</div>
				<div
					on:click={() => (timerRunning = false)}
					class="font-bold text-6xl border-slate-800 border-opacity-20 bg-slate-800 bg-opacity-10  border-[1px] rounded-3xl p-4 w-56 text-center text-slate-100 cursor-pointer transition hover:bg-opacity-30 hover:border-transparent"
				>
					Pause
				</div>
			</div>
			<div
				on:click={() => ((timer = 0), (timerRunning = false))}
				class="w-16 p-2 flex h-min justify-center fill-slate-900 cursor-pointer hover:scale-105 transition hover:shadow-lg rounded-xl hover:bg-slate-200 hover:bg-opacity-30 border-[1px] border-transparent hover:border-slate-500 hover:border-opacity-20"
			>
				<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"
					><!--! Font Awesome Pro 6.2.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path
						d="M463.5 224H472c13.3 0 24-10.7 24-24V72c0-9.7-5.8-18.5-14.8-22.2s-19.3-1.7-26.2 5.2L413.4 96.6c-87.6-86.5-228.7-86.2-315.8 1c-87.5 87.5-87.5 229.3 0 316.8s229.3 87.5 316.8 0c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0c-62.5 62.5-163.8 62.5-226.3 0s-62.5-163.8 0-226.3c62.2-62.2 162.7-62.5 225.3-1L327 183c-6.9 6.9-8.9 17.2-5.2 26.2s12.5 14.8 22.2 14.8H463.5z"
					/></svg
				>
			</div>
		</div>
	</div>
</div>
