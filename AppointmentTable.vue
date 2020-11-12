<template>
<table>
  <thead>
    <tr>
      <th>Guardain Name</th>
      <th>Student Name</th>
      <th>Subject</th>
      <th>Topic</th>
      <th>Appointment</th>
      <th>Delete</th>
      <th>Update</th>
    </tr>
  </thead>
 <div v-if="appointments.length == 0"  class="alert alert-danger" style="width:100%">No Records Available </div>
  <tbody v-if="appointments.length > 0">
    
    
      <tr  v-for="(data,index) in appointments" :key="index">
          <td data-column="Guardian Name">{{data.guardianname}}</td>
          <td data-column="Student Name">{{data.studentname}}</td>
          <td data-column="Subject">{{data.subject}}</td>
          <td data-column="Topic">{{data.topic}}</td>
          <td data-column="appointment">{{data.appointment}}</td>
          <td data-column="appointment"><button style="border-radius:5px;" class="btn btn-danger" @click="deleteRecord(data._id)">Delete</button></td>
          <td data-column="">  <a :href="'appointments/'+data._id" class="btn btn-primary">Update</a></td>
	 
      </tr>

	   
    
  </tbody>

</table> 
 
</template>

<script>
import axios from 'axios'
export default {
    name:'AppointmentTable',
    components:{
      
	},
	created(){
		this.fetchRecords();
	},
	data(){
		return{
			appointments:[]
		}
	},
	methods:{
		fetchRecords(){
			axios.get('http://localhost:5000/api/bookings/',{headers: {
	  'Access-Control-Allow-Origin': '*',
	}}).then(response=>{
				 
				this.appointments = (response.data)
			}).catch(err=>console.log(err))
		},
		deleteRecord(id){
			   axios.delete(`http://localhost:5000/api/bookings/${id}`).then(
				   res=>{
					   console.log(res.data);;
					   this.fetchRecords();
				   }
			   ).catch(err=>{
				   console.log(err)
			   })
		 
		},
		updateRecord(data){

		}
	}
     
}
</script>
<style scoped>
table { 
	width: 100%; 
	border-collapse: collapse; 
	margin-left:20px;
  margin-right: 40px;
	}

/* Zebra striping */
tr:nth-of-type(odd) { 
	background: #eee; 
	}

th { 
	background: #3498db; 
	color: white; 
	font-weight: bold; 
	}

td, th { 
	padding: 10px; 
	border: 1px solid #ccc; 
	text-align: left; 
	font-size: 18px;
	}

/* 
Max width before this PARTICULAR table gets nasty
This query will take effect for any screen smaller than 760px
and also iPads specifically.
*/
@media only screen and (max-width: 760px),
(min-device-width: 768px) and (max-device-width: 1024px){
	table {
    width: 100%; 
	}
	table, thead, tbody, th, td, tr { 
		display: block; 
	}
	
	/* Hide table headers (but not display: none;, for accessibility) */
	thead tr { 
		position: absolute;
		top: -9999px;
		left: -9999px;
	}
	
	tr { border: 1px solid #ccc; }
	
	td { 
		/* Behave  like a "row" */
		border: none;
		border-bottom: 1px solid #eee; 
		position: relative;
		padding-left: 50%; 
	}

	td:before { 
		/* Now like a table header */
		position: absolute;
		/* Top/left values mimic padding */
		top: 6px;
		left: 6px;
		width: 45%; 
		padding-right: 10px; 
		white-space: nowrap;
		/* Label the data */
		content: attr(data-column);

		color: #000;
		font-weight: bold;
	}

}
 
</style>