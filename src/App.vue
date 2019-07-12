<template>
	<div>
		<Countdown :time="60000" />
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
