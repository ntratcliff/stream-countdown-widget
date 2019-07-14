<template>
	<div>
		<Countdown :hours="1" :minutes="15" :seconds="30" />
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
