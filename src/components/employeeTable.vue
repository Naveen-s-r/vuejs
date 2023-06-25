<template>
    <p v-if="employee.length<1">No Data in the Table</p>
    <table v-else>
        <thead>
            <tr>
                <th>Roll.No</th>
                <th>Employee Name</th>
                <th>Employee Email</th>
                <th>Action</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="emp in employee" :key="emp.id">
                <td v-if="editting == emp.id">
                    <input type="text" v-model="emp.id" />
                </td>
                <td v-else>{{ emp.id }}</td>
                <td v-if="editting == emp.id">
                    <input type="text" v-model="emp.name" />
                </td>
                <td v-else>{{ emp.name }}</td>
                <td v-if="editting == emp.id">
                    <input type="text" v-model="emp.email" />
                </td>
                <td v-else>{{ emp.email }}</td>
                <td v-if="editting == emp.id">
                    <button @click="editSavebtn(emp)">Save</button>
                    <button class="delete" @click="cancelEdit(emp)">Cancel</button>
                </td>
                <td v-else>
                    <button @click="editMode(emp)">Edit</button>
                    <button class="delete" @click="$emit('delete:employee',emp.id)">Delete</button>
                </td>
            </tr>
        </tbody>
    </table>
</template>

<script>
export default {
    name:'employee-table',
    props:{
        employee:Array
    },
    data(){
        return{
            editting:null,
            tempData:null,
            emp:{
                id:'',
                name:'',
                email:''
            }
        }
    },
    methods:{
        editMode(emp){
            this.editting = emp.id;
            this.tempData = Object.assign({},emp)
        },
        editSavebtn(emp){
            if(emp.id === ''||emp.name === '' || emp.email === ''){
                return;
            }
            this.$emit('edit:employee',emp.id,emp);
            this.editting = null;
        },
        cancelEdit(emp){
            this.editting = null;
            Object.assign(emp,this.tempData)
        }
    }
}
</script>

<style scoped>
tr{
    text-align: center;
}
button{
    margin-right: 1rem;
}
.delete{
    background-color: red;
    border: 1px solid red;
}
</style>