<template>
	<div class="countdown">
		<span class="time-left hours">{{ hours }}h </span>
		<span class="time-left minutes">{{ minutes }}m </span>
		<span class="time-left seconds">{{ seconds }}s</span>
	</div>
</template>

<script>
const INTERVAL = 1000 // 1 second
const MS_TO_SEC = 1 / 1000 // converts milliseconds to seconds
const SEC_TO_MIN = 1 / 60 // converts seconds to minutes
const MIN_TO_SEC = 1 / SEC_TO_MIN // converts minutes to seconds
const MIN_TO_HR = 1 / 60 // converts minutes to hours
const MS_TO_MIN = MS_TO_SEC * SEC_TO_MIN // converts ms to mins
const MS_TO_HR = MS_TO_MIN * MIN_TO_HR // converts ms to hours
const HR_TO_MIN = 1 / MIN_TO_HR // converts hr to mins

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
			totalMilliseconds: 0
		}
	},
	computed: {
		// Whether or not the timer is currently counting down
		counting () {
			return this.intervalHandle != null
		},

		// The total number of seconds on the clock
		totalSeconds () {
			return this.totalMilliseconds * MS_TO_SEC
		},

		// The total number of minutes on the clock
		totalMinutes () {
			return this.totalMilliseconds * MS_TO_MIN
		},

		// The total number of hours on the clock
		totalHours () {
			return this.totalMilliseconds * MS_TO_HR
		},

		// The total number of seconds on the clock, floored
		totalSecondsInt () {
			return Math.floor(this.totalSeconds)
		},

		// The total number of mintues on the clock, floored
		totalMinutesInt () {
			return Math.floor(this.totalMinutes)
		},

		// The total number of hours on the clock, floored
		totalHoursInt () {
			return Math.floor(this.totalHours)
		},

		// The number of milliseconds, fatctoring in seconds, minutes, and hours
		milliseconds () {
			return this.totalMillisecondsInt - MS_TO_SEC * this.totalSecondsInt
		},

		// The number of seconds, factoring in minutes and hours
		seconds () {
			console.log(this.totalMinutesInt)
			return this.totalSecondsInt - MIN_TO_SEC * this.totalMinutesInt
		},

		// The number of minutes, factoring in hours
		minutes () {
			return this.totalMinutesInt - HR_TO_MIN * this.totalHoursInt
		},

		// The number of hours remaining
		hours () {
			return Math.floor(this.totalMilliseconds * MS_TO_HR)
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
			this.totalMilliseconds = milliseconds
			console.log(milliseconds)
		},
		// Decrements the number of seconds left by the interval
		update (interval) {
			this.totalMilliseconds = Math.max(this.totalMilliseconds - interval, 0)

			if (this.totalMilliseconds === 0) {
				this.stop()
			}
		}
	}
}
</script>
