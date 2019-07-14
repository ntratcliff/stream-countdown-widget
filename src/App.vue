<template>
	<div>
		<Countdown
			:hours="0"
			:minutes="0"
			:seconds="5"
			:hide-empty-units="true"
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
