<template>
    <span>{{weatherCondition}} <fa :icon=getIcon() /></span>
</template>

<script>
export default {
    props: ['weatherCondition'],
    data() {
        return {}
    },
    methods: {
        getIcon() {
            const mistyConditions = ['Mist', 'Smoke', 'Haze', 'Dust', 'Fog', 'Sand', 'Dust', 'Ash', 'Squall', 'Tornado']
            var status = this.weatherCondition
            if(status == "Thunderstorm") {
                return 'bolt'
            }
            else if(status == "Drizzle" || status == "Rain") {
                return 'cloud-rain'
            }
            else if(status == "Snow") {
                return 'snowflake'
            }
            else if(status == "Clear") {
                return 'sun'
            }
            else if(status == "Clouds") {
                return 'cloud'
            }
            else if(mistyConditions.includes(status)) {
                return 'smog'
            }
        }
    },
}
</script>
