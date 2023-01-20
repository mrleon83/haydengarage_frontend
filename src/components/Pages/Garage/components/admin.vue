<script>
import axios from 'axios'
import Datepicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css'

export default {
  components: { Datepicker },
  data() {
    return {
      date: null,
      appointments: [],
      dateBlock: null
    }
  },
  methods: {    
    getAppointments(){
        axios.post('http://localhost/api/getappointments', {
            date: this.date,
            time: this.time,
        })
        .then((response) => {
            console.log(response)
            this.appointments = response.data.data
        })
        .catch(function (error) {
            console.log(error);
        });
    },
    blockappointments(){
        axios.post('http://localhost/api/blockappointments', {
            date: this.dateBlock,
        })
        .then((response) => {
            console.log(response)
            this.appointments = response.data.data
        })
        .catch(function (error) {
            console.log(error);
        });
    }
  },
}
</script>

<template>
    <div>
        <hr/>
            <form @submit.prevent="submit" class="appointmentp-form">
             <h2>Search appointments by date <b></b> </h2>
                <Datepicker v-model="date" day-picker :enable-time-picker="false" @update:modelValue="getAppointments"></Datepicker>
                <div>
                   
                    <ul v-for="appointment in appointments" :key="appointment.id">
                        <li>
                            {{ appointment.day }}/{{ appointment.month }}/{{ appointment.year }}
                            {{ appointment.time }}
                            {{ appointment.vehiclemake }} - 
                            {{appointment.vehiclemodel}} 
                            {{ appointment.customer }}</li>
                        
                    </ul>
                </div>
            </form>

            <form @submit.prevent="blockappointments" class="appointmentp-form">
             <h2>Block appointments<b></b> </h2>
                    <Datepicker v-model="dateBlock" day-picker :enable-time-picker="false"></Datepicker>
                    {{ dateBlock }}
                <div>
                    <button style="background-color: red;">Block appointments for {{ dateBlock }}</button>
                </div>
            </form>

        </div>

</template>