<template>
    <div>
        <form @submit.prevent="submitData">
            <label>Employee name *</label>
            <input type="text" v-model="employeeObj.name"
            :class="{'has-error':submitting&&InvalidName}"
            @focus="clearStatus"
            @keypress="clearStatus"
            ref="firstInput"
            />
            <label>Employee email *</label>
            <input type="email" v-model="employeeObj.email" 
            :class="{'has-error':submitting&&InvalidEmail}"
            @focus="clearStatus"
            @keypress="clearStatus"
            />
            <p v-if="submitting&&error" class="error-msg">Fill all required fields</p>
            <p v-if="success" class="success-msg">Employee added Successfully</p>
            <button>submit</button>
        </form>
    </div>
</template>
<script>
export default {
    name:'employee-table',
    data(){
        return{
            error:false,
            success:false,
            submitting:false,
            employeeObj:{
                name:'',
                email:''
            }
        }
    },
    methods:{
        submitData(){
            this.submitting=true;
            this.clearStatus();

            if(this.InvalidEmail || this.InvalidName){
                this.error=true;
                return;
            }
            this.$emit('add:emp',this.employeeObj);
            this.$refs.firstInput.focus();
            this.employeeObj = {
                name:'',
                email:''
            }
            this.success = true;
            this.submitting = false;
            this.error = false;
        },
        clearStatus(){
            this.error=false;
            this.success=false;
        }
    },
    computed:{
        InvalidName(){
        return this.employeeObj.name === '';
        },
        InvalidEmail(){
        return this.employeeObj.email === '';
        }
    }
}
</script>
<style scoped>
form{
    margin: 2rem;
}
.success-msg{
    color:green;
    margin: 20px 0;
}
.error-msg{
    color:red;
    margin: 20px 0;
}

</style>