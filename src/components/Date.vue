<template>
    <span v-if="standard=='true'">{{fetchCurrentDate()}}</span>
    <span v-else> {{fetchFormatAMPM()}}</span>
</template>

<script>
export default {
    props: ['dateObject', 'standard'],
    methods: {
        fetchCurrentDate() {
            var date = this.dateObject
            var day = String(date.getDate()).padStart(2, '0');
            var month = String(date.getMonth() + 1).padStart(2, '0'); //January is 0!
            var year = date.getFullYear();
            const monthNames = ["January", "February", "March", "April", "May", "June",
            "July", "August", "September", "October", "November", "December"];
            date =  day + " " + monthNames[parseInt(month) - 1] + ", " + year;
            return date;
        },
        fetchFormatAMPM() {
            var date = this.dateObject
            var hours = date.getHours();
            var minutes = date.getMinutes();
            var ampm = hours >= 12 ? 'pm' : 'am';
            hours = hours % 12;
            hours = hours ? hours : 12; // the hour '0' should be '12'
            minutes = minutes < 10 ? '0'+minutes : minutes;
            return hours + ':' + minutes + ' ' + ampm;
        },
    },
}
</script>

