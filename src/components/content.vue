<template>
    <div class="content-wrapper">
        <div class="content">
            <short-data></short-data>
        </div>
        
    </div>
</template>
<script>
import shortdata from './dataTemplate/shortData'
export default {
  name: 'appcontent',
  components: {
    'short-data':shortdata,
  }
}
</script>
<style  scoped>
    .content-wrapper{
        float: left;  
        width: calc(100% - 200px);
        padding-left: 200px;
    }
    .content{
        background-color:#EEFAF5; 
        min-height:1000px ;
    }
</style>
