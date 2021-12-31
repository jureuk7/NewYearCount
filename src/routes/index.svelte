<script lang="ts">
	import { fly } from 'svelte/transition';
	import { onMount } from 'svelte';

	const endDate = new Date('01/01/2022 00:00 AM');
	let endView = false;
	let date = {};
	let timer;

	const displayCount = () => {
		const day: Date = new Date();
		if (endDate - day < 0) {
			clearInterval(timer);
			endView = !endView;
		}
		let newSec = Math.floor(((endDate - day) % (1000 * 60)) / 1000);
		date.sec = newSec;
		let newMin = Math.floor(((endDate - day) % (1000 * 60 * 60)) / (1000 * 60));
		date.min = newMin;
		let newHour = Math.floor(((endDate - day) % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
		date.hour = newHour;
	};

	onMount(() => {
		timer = setInterval(displayCount, 1000);
	});
</script>

<main>
	<h1 transition:fly={{ x: 50 }}>2022년까지 남은 시간</h1>

	{#if endView}
		<div class="end" transition:fly={{ x: 50 }}>새해가 밝았습니다 ! 모두 새해 복 많이받으세요~</div>
	{:else}
		<div class="count">
			{date.hour}시간 {date.min}분 {date.sec}초
		</div>
	{/if}
</main>

<style lang="css">
	@import url('https://cdn.jsdelivr.net/gh/orioncactus/pretendard/dist/web/static/pretendard.css');
	main {
		position: absolute;
		display: flex;
		width: 100%;
		height: 100%;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		font-family: Pretendard;
	}
	h1 {
		font-weight: 700;
		color: orange;
	}
	.count {
		color: black;
		font-weight: 800;
		font-size: 50px;
	}
	.end {
		color: black;
		font-weight: 700;
		font-size: 60px;
	}
</style>
