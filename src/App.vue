<script>
import CreateAppointment from './components/Pages/Garage/components/createAppointment.vue'
import Admin from './components/Pages/Garage/components/admin.vue'
export default {
  components: { CreateAppointment, Admin  },
  data() {
    return {
      showBooking: false,
      showAdmin: false
    }
  },
  methods: {  
    showBookingForm(){
      this.showBooking = true
      this.showAdmin = false
    },
    showAdminForm(){
      this.showBooking = false
      this.showAdmin = true
    }
  },

}
</script>

<template>
  <div style="width: 100%">
    <button @click="showBookingForm()" class="admin-btn">Customer Booking</button>
    <button @click="showAdminForm()" class="admin-btn">Admin</button>
    <div v-if="showBooking">
      <CreateAppointment/>
    </div>
    <div v-if="showAdmin">
      <Admin/>
    </div>
    
    
  </div>

</template>

<style scoped>

</style>
