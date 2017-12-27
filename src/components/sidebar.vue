<template>
   <ul class="slidUl menu">
        <li class="one">
            <a href="javascript:;">1</a>
            <ul class="slidUl slid1">
                <li class="two">
                    <a href="javascript:;">2</a>
                    <ul class="slidUl slid2">
                        <li class="three"><a href="javascript:;">3</a></li>
                    </ul>
                </li>
            </ul>
        </li>
        <li class="one">
            <a href="javascript:;">1</a>
            <ul class="slidUl slid1">
                <li class="two">
                    <a href="javascript:;">2</a>
                    <ul class="slidUl slid2">
                        <li class="three"><a href="javascript:;">3</a></li>
                    </ul>
                </li>
            </ul>
        </li>
    </ul>
</template>
<script>

export default {
  name: 'appsiderbar'
}
</script>

<style  scoped>

.slidUl{
    width: 200px;
    height: auto;
    background-color: rgba(38, 52, 75, 1);
    display: none;  
    position: fixed;
    left: 200px;
    top:0px;
}
.menu{
    display: block;
    width: 200px;
    top: 73px;
    z-index:2;
    left: 0;
    float: left;
}
.menu li{
    list-style: none; 
    width: 200px;
    height: 46px;
    text-align: left;
    padding-left: 20px;
    line-height: 46px;
    position: relative;
}
.menu li:hover{
    background: #212d41;
    color: #e4e9f2;
}
.menu li a{
    color: #c3cee0;
    text-decoration: none;
}
.menu li:hover{
    text-decoration: none;
}
  
.menu li.one:hover .slid1{
    display: block;
}
.menu li.two:hover .slid2{
    display: block;
}




</style>
