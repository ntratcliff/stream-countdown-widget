<template>
	<div class="countdown">
		<span
			v-if="displayHours && (!hideEmptyUnits || totalHoursInt >= 1)"
			class="hours"
		>
			<span class="time">{{ currentHours }}</span><span class="text">{{ hoursText }} </span>
		</span>
		<span
			v-if="!hideEmptyUnits || totalMinutesInt >= 1"
			class="minutes"
		>
			<span class="time">{{ currentMinutes }}</span><span class="text">{{ minutesText }} </span>
		</span>
		<span
			v-if="displaySeconds"
			class="seconds"
		>
			<span class="time">{{ currentSeconds.toString().padStart(padSeconds ? 2 : 0, '0') }}</span><span class="text">{{ secondsText }} </span>
		</span>
	</div>
</template>

<script>
const INTERVAL = 1000 // 1 second
const MS_TO_SEC = 1 / 1000 // converts milliseconds to seconds
const SEC_TO_MIN = 1 / 60 // converts seconds to minutes
const MIN_TO_HR = 1 / 60 // converts minutes to hours

const MS_TO_MIN = MS_TO_SEC * SEC_TO_MIN // converts ms to mins
const MS_TO_HR = MS_TO_MIN * MIN_TO_HR // converts ms to hours
const SEC_TO_MS = 1 / MS_TO_SEC // converts sec to ms
const MIN_TO_MS = 1 / MS_TO_MIN // converts min to ms
const MIN_TO_SEC = 1 / SEC_TO_MIN // converts minutes to seconds
const HR_TO_MIN = 1 / MIN_TO_HR // converts hr to mins
const HR_TO_MS = 1 / MS_TO_HR // converts hr to ms

export default {
	props: {
		// Time in hours
		hours: {
			type: Number,
			required: false,
			default: 0
		},
		minutes: {
			type: Number,
			required: false,
			default: 0
		},
		// Time in seconds
		seconds: {
			type: Number,
			required: false,
			default: 0
		},
		hoursText: {
			type: String,
			required: false,
			default: 'h '
		},
		minutesText: {
			type: String,
			required: false,
			default: 'm '
		},
		secondsText: {
			type: String,
			required: false,
			default: 's '
		},
		// Hides units that don't have a value >=1 (i.e. no hours disply for 59 minutes)
		hideEmptyUnits: {
			type: Boolean,
			required: false,
			default: false
		},
		displayHours: {
			type: Boolean,
			required: false,
			default: true
		},
		// TODO: display minutes
		displaySeconds: {
			type: Boolean,
			required: false,
			default: true
		},
		// TODO: pad minutes and hours
		// whether or not to add padding 0s to the second counter
		padSeconds: {
			type: Boolean,
			required: false,
			default: true
		}
	},
	data: function () {
		return {
			intervalHandle: null,
			time: 0
		}
	},
	computed: {
		// Whether or not the timer is currently counting down
		counting () {
			return this.intervalHandle != null
		},

		totalMilliseconds () {
			return this.time
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
		currentMilliseconds () {
			return this.totalMillisecondsInt - MS_TO_SEC * this.totalSecondsInt
		},

		// The number of seconds, factoring in minutes and hours
		currentSeconds () {
			return this.totalSecondsInt - MIN_TO_SEC * this.totalMinutesInt
		},

		// The number of minutes, factoring in hours
		currentMinutes () {
			var hours = this.displayHours ? this.totalHoursInt : 0
			return Math.max(this.totalMinutesInt - HR_TO_MIN * hours, 0)
		},

		// The number of hours remaining
		currentHours () {
			return Math.floor(this.totalMilliseconds * MS_TO_HR)
		}
	},
	created () {
		// compute time from h m s
		this.time += this.hours * HR_TO_MS
		this.time += this.minutes * MIN_TO_MS
		this.time += this.seconds * SEC_TO_MS

		this.set(this.time)
	},
	mounted () {
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
			this.time = milliseconds
			console.log(milliseconds)
		},
		// Decrements the number of seconds left by the interval
		update (interval) {
			this.time = Math.max(this.totalMilliseconds - interval, 0)

			if (this.time === 0) {
				this.stop()
			}
		}
	}
}
</script>
