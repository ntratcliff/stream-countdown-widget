<template>
	<div class="countdown">
		<span class="time-left">{{ secondsInt }}</span>
	</div>
</template>

<script>
const INTERVAL = 1000 // 1 second
const MS_TO_SEC = 1 / 1000 // converts milliseconds to seconds
const SEC_TO_MIN = 1 / 60 // converts seconds to minutes
const MIN_TO_HR = 1 / 60 // converts minutes to hours
const MS_TO_MIN = MS_TO_SEC * SEC_TO_MIN // converts ms to mins
const MS_TO_HR = MS_TO_MIN * MIN_TO_HR // converts ms to hours

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
			return this.milliseconds * MS_TO_SEC
		},

		minutes () {
			return this.milliseconds * MS_TO_MIN
		},

		hours () {
			return this.milliseconds * MS_TO_HR
		},

		secondsInt () {
			return Math.floor(this.seconds)
		},

		minutesInt () {
			return Math.floor(this.minutes)
		},

		hoursInt () {
			return Math.floor(this.hours)
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
