<template>
    <div v-if="isOpen" class="backdrop" @click="false">
        <div class="popup">
            <form>
            <h4>Add Contact</h4>
            <input v-model="name" required pattern="[A-Za-z]+" type="text" placeholder="Name">
            <input v-model="mobilePhone" required pattern="\+\d{3}-\d{2}-\d{3}-\d{2}-\d{2}"  type="text" placeholder="Mobile phone">
            <input v-model="jobTitle" required pattern="[A-Za-z]+" type="text" placeholder="Job Title">
            <input v-model="birthDate" required type="date" placeholder="Date of birth" >
            <button type="submit" @click="addContact">Add</button>
            <button @click="close">Cancel</button>    
        </form>   
        </div>           
    </div>
</template>
 
<script>
const axios = require('axios').default;
    export default {
        data(){
            return{
            ruleName:[
                (v)=>!v || /[A-Za-z]/.test(v) || "Symbols",
            ]
            }
        },
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

            fetchContacts(){
        axios
            .get('https://localhost:44374/api/Contact')
            .then(resp => {
                this.contacts = resp.data
                console.log(resp.data);           
            }); 
            },

            addContact(){
        axios        
            .post('https://localhost:44374/api/Contact',{
                    Name: this.name,
                    MobilePhone: this.mobilePhone, 
                    JobTitle: this.jobTitle,
                    BirthDate: this.birthDate})
                    .then(() => {fetchContacts()});
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
            },
            validateInput(input) {
            if (input.value.length < 3) {
                input.setCustomValidity("Min length = 3.Correct pls");   
            }
            else {
                // Длина комментария отвечает требованию, 
                // поэтому очищаем сообщение об ошибке
                input.setCustomValidity("");
            }
            }
        }
        
    };
</script>
 
<style>
    .popup {
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

.backdrop {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.8);
  z-index: 100;
}
</style>