<template>
    <tr>
        <td>{{ manufacturer.manufacturerId }}</td>
        <td>{{ manufacturer.name }}</td>
        <td><button id="editBtn" type="button"
                  class="inline-flex items-center px-4 py-2 border border-gray-300 rounded-md shadow-sm text-sm font-medium text-gray-700 bg-blue-400 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-400"
                  @click="updateManufacturer">
            <svg class="-ml-1 mr-2 h-5 w-5 text-gray-500" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"
                 fill="currentColor" aria-hidden="true"><path
              d="M13.586 3.586a2 2 0 112.828 2.828l-.793.793-2.828-2.828.793-.793zM11.379 5.793L3 14.172V17h2.828l8.38-8.379-2.83-2.828z"/>
            </svg>
            Edit
        </button></td>
        
        <td><button id="delBtn" type="button"
         class="inline-flex items-center px-4 py-2 border border-gray-300 rounded-md shadow-sm text-sm font-medium text-gray-700 bg-red-400 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-400"
         @click="deleteManufacturer" >
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zM8.707 7.293a1 1 0 00-1.414 1.414L8.586 10l-1.293 1.293a1 1 0 101.414 1.414L10 11.414l1.293 1.293a1 1 0 001.414-1.414L11.414 10l1.293-1.293a1 1 0 00-1.414-1.414L10 8.586 8.707 7.293z" clip-rule="evenodd" />
            </svg>
            Delete
        </button>
        </td>
    </tr>
</template>

<script>
export default{
methods: {
    deleteManufacturer(){
        this.$swal({
  title: 'Are you sure?',
  text: "You won't be able to revert this!",
  icon: 'warning',
  showCancelButton: true,
  confirmButtonColor: '#3085d6',
  cancelButtonColor: '#d33',
  confirmButtonText: 'Yes, delete it!'
    }).then((result) => {
    if (result.isConfirmed) {
        this.$axios
            .delete('https://localhost:44319/api/Manufacturers',{
            data:{
                manufacturerId:this.manufacturer.manufacturerId,
            },
        })
        .then((response)=> {
            this.$emit('delete-event', this.manufacturer.manufacturerId)
            })
            this.$swal('Deleted!', 'success')
            }
        })
    },

    async updateManufacturer() {
      const { value: updatedManufacturer } = await this.$swal({
        title: 'Enter new manufacturer name',
        input: 'text',
        showCancelButton: true,
        inputValidator: (value) => {
          if (!value) {
            return 'You need to write something!'
          }
        },
      })
      if (updatedManufacturer) {
        this.$axios
          .put('https://localhost:44319/api/Manufacturers', {
            ManufacturerId: this.manufacturer.manufacturerId,
            name: updatedManufacturer,
          })
          .then((response) => {
            this.$emit('update-event', {
              manufacturerId: this.manufacturer.manufacturerId,
              name: updatedManufacturer
            })
          })
        this.$swal(`The new manufacturer name is ${updatedManufacturer}`)
        }
    },
},
props: {
    manufacturer: {
      type: Object,
      required: true,
        }
    }
}
</script>

<style>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}
td, th {
  border: 1px solid #bbbbbb;
  text-align: left;
  padding: 8px;
}
tr:nth-child(even) {
  background-color: #e3e3e3;
}
</style>
