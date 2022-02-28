<template>
     <h2>Contact list page</h2>           
        <div>       
        <table class="table table-striped" v-if="contacts.length">            
            <tr>
                <th >
                    Contact Id
                </th>
                <th >
                    Name
                </th>
                <th >
                    Mobile phone
                </th>
                <th >
                    Job title
                </th>
                <th >
                    Date of birth
                </th>
                <th >
                    Options
                </th>
            </tr>  
            <tr  v-for="(contact,i) in  contacts" :key="i">
                <td>{{contact.Id}}</td>
                <td>{{contact.Name}}</td>  
                <td>{{contact.MobilePhone}}</td>  
                <td>{{contact.JobTitle}}</td>  
                <td>{{contact.BirthDate}}</td>  
                <td>
                    <input @click="deleteContact(contact.Id)" class="buttonDelete" type="submit" value="Delete">
                    <button class="buttonEdit" @click="isPopupOpen1 = true;getContactById(contact.Id)">Edit</button>
                    <div id = 'tag-id' style = 'display: block;'>
                         <popup-edit
                        :is-open="isPopupOpen1"
                        @close="isPopupOpen1=false">
                        <h5>Confirm id {{contactEdit.Id}}!</h5><br/>
                        </popup-edit> 
                     </div>  
                </td>      
            </tr> 
        </table> 
        <p v-else>Коллекция пуста</p>
        <button class="button" @click="isPopupOpen = true">Add contact</button>
        <popup-add
        :is-open="isPopupOpen"
        @ok="popupConfirmed"
        @close="isPopupOpen=false"></popup-add>        
        </div>
</template>


<script>
import PopupEdit from './PopupEdit.vue';
import PopupAdd from './PopupAdd.vue';
const axios = require('axios').default;
export default{
    components: {
        PopupAdd,PopupEdit
    }, 
    props:{
        id:{
            type:Number
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
    data() {
        return {
            isPopupOpen:false,
            isPopupOpen1:false,
            contacts:[], 
            contactEdit:[]                
        }
    },
    mounted(){
        axios
            .get('https://localhost:44374/api/Contact')
            .then(resp => {
                this.contacts = resp.data
                console.log(resp.data);
            });       
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
    getContactById(id){
        axios
            .get('https://localhost:44374/api/Contact/' + id)
            .then(resp => {
                this.contactEdit = resp.data
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
    editContact(){
        axios        
            .put('https://localhost:44374/api/Contact',{
             Name: this.name,
             MobilePhone: this.mobilePhone, 
             JobTitle: this.jobTitle,
             BirthDate: this.birthDate})
            .then(() => {fetchContacts()});
            
    },
    deleteContact(id){        
        axios
            .delete('https://localhost:44374/api/Contact/' + id)
            .then(()=>this.fetchContacts())
            .catch(error => {
            alert(error); // Error: Not Found
        });
    },    
    openPopup() {
      this.confirmation = "";
      this.isPopupOpen = true;
    },
    openPopup1(id) {
      this.confirmation = "";
      this.isPopupOpen1 = true;
      
    },
    PopupConfirmed1(){
        alert("Confirmed!");
        this.isPopupOpen1 = false;
    },
    getId(id)
    {
        this.id = id;
    }
}
}

</script>

<style >
     .button{
        float: left;
        margin-block-end: 20px;
    }
    .buttonDelete{
        width: 90px;
        float: left;
    }
    .buttonEdit{
        float: left;
        width: 90px;
    }
    #tag-id{
display: none;
}
</style>