<template>
    <div class="app">
        <contact-list />
    </div>    
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">
</template>

<script>
import ContactList from './components/ContactList.vue';
const axios = require('axios').default;
export default {
  components: {
       ContactList
    },     
    data() {
        return {
                               
        }
    },  
    methods:{

    }
}
</script>

<style>
    .app{
        padding: 20px;
        margin-top:0px;
    }      

</style>