<template>
	<div>
		<Countdown
			:hours="1"
			:minutes="15"
			:seconds="5"
			:hide-empty-units="true"
			:display-hours="false"
			minutes-text=":"
			seconds-text=""
		/>
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
