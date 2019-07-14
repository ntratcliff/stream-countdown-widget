<template>
	<div class="root">
		<Countdown
			:hours="hours"
			:minutes="minutes"
			:seconds="seconds"
			:display-hours="false"
			minutes-text=":"
			seconds-text
		/>
		<span class="minutes-text">minutes</span>
	</div>
</template>

<script>
import Countdown from './components/Countdown.vue'

export default {
	components: {
		Countdown
	},
	data () {
		return {
			hours: 0,
			minutes: 0,
			seconds: 0
		}
	},
	created () {
		// get hms from query string
		let params = new URLSearchParams(window.location.search.substring(1))

		if (params.get('h') != null) {
			this.hours = parseInt(params.get('h'))
		}

		if (params.get('m') != null) {
			this.minutes = parseInt(params.get('m'))
		}

		if (params.get('s') != null) {
			this.seconds = parseInt(params.get('s'))
		}
	}
}
</script>
