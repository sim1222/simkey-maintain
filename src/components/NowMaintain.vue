<template>
	<div>
		<h1>メンテナンス中</h1>
		<h3>残り予想: {{ relativeTime }}</h3>
		<p>サーバーステータスは<a href="https://status.sim1222.com">こちら</a></p>
	</div>
</template>

<script setup lang="ts">
import { computed, onMounted, ref } from 'vue';

const endTime = parseInt(import.meta.env.VITE_END_TIME) ?? 1600008033008;
const now = ref(Date.now());
const relativeTime = computed(() => {
	const diff = endTime - now.value;
	const sec = Math.floor(diff / 1000);
	const min = Math.floor(sec / 60);
	const hour = Math.floor(min / 60);
	const day = Math.floor(hour / 24);
	const month = Math.floor(day / 30);
	const year = Math.floor(month / 12);

	if (year > 0) return `${year}年`;
	if (month > 0) return `${month}ヶ月`;
	if (day > 0) return `${day}日`;
	if (hour > 0) return `${hour}時間`;
	if (min > 0) return `${min}分`;
	if (sec > 0) return `${sec}秒`;
	if (diff > 0) return `${diff}ミリ秒`;
	if (diff === 0) return `0`;
	// show minus if diff is negative
	if (-1000 < diff) return `${diff}ミリ秒`;
	if (-60000 < diff) return `${sec}秒`;
	if (-3600000 < diff) return `${min}分`;
	if (-86400000 < diff) return `${hour}時間`;
	if (-2629800000 < diff) return `${day}日`;
	if (-31557600000 < diff) return `${month}ヶ月`;
	return `${year}年`;
});

onMounted(() => {
	//update time every frame using requestAnimationFrame
	const update = () => {
		now.value = Date.now();
		requestAnimationFrame(update);
	};
	requestAnimationFrame(update);
});
</script>

<style lang="scss" module></style>
