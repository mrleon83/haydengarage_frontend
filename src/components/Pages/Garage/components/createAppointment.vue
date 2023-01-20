<script>
import axios from 'axios'
import Datepicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css'

export default {
  components: { Datepicker },
  data() {
    return {
      form: {
        name: null,
        email: null,
        phone: null,
        make: null,
        model: null,
      },
      msg: '',
      customerId: null,
      emailOk: false,
      date: null,
      time: null,
      selectedTime: null,
      showTimes: false,
      availableTimes: [],
      showCustomer: true,
      showConfirmation: false,
      bookingConfirmation: {}
    }
  },
  methods: {
    validateEmail() {
        if (/^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(this.form.email)) {
            this.msg = ''
            this.emailOk = true
        } else {
            this.msg = 'Please enter a valid email address';
            
        }
    },
    
    submit() {
        let name = this.form.name
        let email = this.form.email
        let phone = this.form.phone
        let make = this.form.make
        let model = this.form.model
        axios.post('http://localhost/api/savecustomer', {
            name: name,
            email: email,
            phone: phone,
            make: make,
            model: model
         })
        .then((response) => {
            this.customerId = response.data.id
            this.showCustomer = false
            this.showTimes = true
        })
        .catch(function (error) {
            console.log(error);
        });
    },

    handleDate() {
        this.availableTimes = []
        axios.post('http://localhost/api/checktimes', {
            date: this.date
        })
        .then((response) => {
            let data = response.data
            for(var o in data) {
                    this.availableTimes.push(data[o]);
            }
            
        })
        .catch(function (error) {
            console.log(error);
        });
    },

    clickedTime(time){
        this.time = time
    },
    
    sendAppointment(){
        if( this.emailOk ) {
            axios.post('http://localhost/api/setappointment', {
            date: this.date,
            time: this.time,
            customerId: this.customerId
        })
        .then((response) => {
            console.log(response)
            this.bookingConfirmation = response.data;
            this.showTimes = false
            this.showConfirmation = true
        })
        .catch(function (error) {
            console.log(error);
        });
        }  
    },
  },

}
</script>
<template>
    <div>
  <h1 class="text-center">Create a customer profile</h1>

    <div class="customer-data" v-if="showCustomer">
        <form @submit.prevent="submit" class="new-customer-form">
                <div class="input-container">
                    <label for="name" class="new-customer-label">Name:</label>
                    <input id="name" v-model="form.name" />
                </div>

                <div class="input-container">
                    <label for="email" class="new-customer-label">Email:</label>
                    <input type="email" placeholder="Please enter your email here" required v-model="form.email" @blur="validateEmail" />
                    <span class="floating-placeholder" v-if="msg">{{msg}}</span>
                </div>

                <div class="input-container">
                    <label for="phone" class="new-customer-label">Phone:</label>
                    <input id="phone" v-model="form.phone" />
                </div>

                <div class="input-container">
                    <label for="make" class="new-customer-label">Make:</label>
                    <input id="make" v-model="form.make" />
                </div>

                <div class="input-container">
                    <label for="model" class="new-customer-label">Model:</label>
                    <input id="model" v-model="form.model" />
                </div>

                <button type="submit" class="next-btn">Save my details</button>
            </form>
        </div>

    <div class="appointment-data" v-if="showTimes && !showConfirmation">
        <div>
            <h1 class="text-center">Book an appointment</h1>

            <form @submit.prevent="submit" class="new-customer-form">
                <Datepicker v-model="date" day-picker :enable-time-picker="false" @update:modelValue="handleDate"></Datepicker>

                <div>
                    <h2>Now Choose a time <b>{{ time }}</b> </h2>
                    <div v-for="time in availableTimes" :key="time.id" class="time-picker-container">
                        <div @click="clickedTime(time)" style="cursor: pointer;" class="time-picker-input">{{ time }}</div>
                    </div>
                    <button type="submit" class="next-btn" @click="sendAppointment">Book appointment</button>
                </div>

            </form>
            </div>
    </div>

    <div class="booking-confirmation" v-if="showConfirmation"> 
        <h3>Booking Confirmed</h3>
        <p>{{ bookingConfirmation.day }}/{{ bookingConfirmation.month }}/{{ bookingConfirmation.year }} at {{ bookingConfirmation.time }}</p>
    </div>


    </div>
</template>

