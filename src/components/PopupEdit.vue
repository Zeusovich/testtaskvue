<template>
    <div v-if="isOpen" class="backdropok" @click="false">
        <div class="popupok">
            <form>
            <h4>Edit Contact</h4>
            <slot></slot> 
            <input v-model.trim="id" required min=1 type="number"  placeholder="Confirm Id">            
            <input v-model.trim="name"  required pattern="[A-Za-z]+"  type="text"  placeholder="Name" >
            <input v-model.trim="mobilePhone" required pattern="\+\d{3}-\d{2}-\d{3}-\d{2}-\d{2}"  type="text"  placeholder="+375-11-111-11-11" >
            <input v-model.trim="jobTitle" required pattern="[A-Za-z]+"  minlength="3" maxlength="30" type="text"  placeholder="Job Title">
            <input v-model="birthDate" required type="date" placeholder="Date of birth" >           
            <button type="submit" @click="editContact">Edit</button>
            <button @click="close">Cancel</button> 
        </form>   
        </div>           
    </div>
</template>
 
<script>
const axios = require('axios').default;
import { required,minLength,maxLength,numeric} from '@vuelidate/validators'
import useVuelidate from '@vuelidate/core'

    export default {        
        props: {
            isOpen: {
                type:Boolean,
                required :true,
            },
                name: {
                type: String,
                required: true
        },
            mobilePhone: {
                type: String,
                required: true
        },
            jobTitle: {
                type: String,
                required: true
        },
            birthDate: {
                type: new Date(),
                required: true
        }      
        },
        /*setup: () => ({ v$: useVuelidate() }),*/
        validations(){
            return {
            xcvid: { required,numeric,maxLength:maxLength(3) },
            xcvname: { required,minLength:minLength(6) } ,
            xcvmobilePhone: { required,maxLength:maxLength(12) }, 
            xcvxjobTitle: { required },
            xcvbirthDate:{required }                 
            }            
        }, 
        emits: {
            ok:null,
            close:null,
        },         
        mounted() {
            document.addEventListener("keydown", this.handleKeydown);
        },
        beforeUnmount() {
            document.removeEventListener("keydown", this.handleKeydown);
        },
        methods:{
            submit(){
                this.v$.$touch;
            },
            fetchContacts(){
        axios
            .get('https://localhost:44374/api/Contact')
            .then(resp => {
                this.contacts = resp.data
                console.log(resp.data);           
            }); 
            },

            editContact(){
                axios        
                    .put('https://localhost:44374/api/Contact',{
                    Id:this.id,
                    Name: this.name,
                    MobilePhone: this.mobilePhone, 
                    JobTitle: this.jobTitle,
                    BirthDate: this.birthDate
                    })
                    .then(() => {fetchContacts()
                    console.log(Name)
                    });  
            },
            handleKeydown(e) {
                if (this.isOpen && e.key === "Escape") {
                    this.close();
                }
            },
            close(){
                this.$emit("close");
            },
            confirm(){
                this.$emit("ok");
            }            
        }               
    };
</script>
 
<style>
    .popupok {
  top: 50px;
  padding: 20px;
  left: 35%;
  transform: translateX(-50%);
  position: fixed;
  z-index: 101;
  background-color: white;
  border-radius: 10px;
}

.popup h1 {
  text-align: center;
  margin: 0;
}

.backdropok {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.151);
  z-index: 100;
}

</style>