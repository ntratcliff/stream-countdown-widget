<template>
	<div class="countdown">
		<span class="time-left">{{ seconds }}</span>
	</div>
</template>

<script>
const INTERVAL = 1000 // 1 second

export default {
	props: {
		// Time in milliseconds
		time: {
			type: Number,
			required: true
		}
	},
	data: function () {
		return {
			intervalHandle: null,
			milliseconds: 0
		}
	},
	computed: {
		counting () {
			return this.intervalHandle != null
		},
		seconds () {
			return Math.floor(this.milliseconds / 1000)
		}
	},
	watch: {
		time (newValue, oldValue) {
			this.set(newValue)
			this.start(INTERVAL)
		}
	},
	mounted () {
		this.set(this.time)
		this.start(INTERVAL)
	},
	methods: {
		// Starts the timer
		start (interval) {
			if (this.counting) {
				this.stop()
			}

			this.intervalHandle = setInterval(
				() => this.update(interval),
				interval
			)
		},
		// Stops the timer if it was running
		stop () {
			if (this.counting) {
				window.clearInterval(this.intervalHandle)
			}
		},
		// Sets the timer value
		set (milliseconds) {
			this.milliseconds = milliseconds
			console.log(milliseconds)
		},
		// Decrements the number of seconds left by the interval
		update (interval) {
			this.milliseconds = Math.max(this.milliseconds - interval, 0)

			if (this.milliseconds === 0) {
				this.stop()
			}
		}
	}
}
</script>
