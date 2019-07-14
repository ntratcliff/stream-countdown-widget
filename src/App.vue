<template>
	<div class="root">
		<Countdown
			:hours="0"
			:minutes="15"
			:seconds="0"
			:display-hours="false"
			minutes-text=":"
			seconds-text=""
		/>
		<span class="minutes-text">minutes</span>
	</div>
</template>

<script>
import Countdown from './components/Countdown.vue'

export default {
	components: {
		Countdown
	}
}
</script>
