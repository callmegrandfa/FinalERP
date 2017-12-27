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

<style lang="scss" scoped>
$background:rgba(38, 52, 75, 1);
$menuColor:#c3cee0;
.slidUl{
    width: 200px;
    height: auto;
    background-color: $background;
    display: none;  
    position: absolute;
    left: 200px;
    top:0px;
}
.menu{
    float: left;
    display: block;
    position: fixed;
    top:73px;
    left: 0;
    li{
        list-style: none; 
        width: 200px;
        height: 46px;
        text-align: left;
        padding-left: 20px;
        line-height: 46px;
        position: relative;
        &:hover{
            background: #212d41;
            color: #e4e9f2;
        }
        a{
            color: $menuColor;
            text-decoration: none;
        }
        &:hover{
            text-decoration: none;
        }
    }
    li.one:hover .slid1{
        display: block;
    }
    li.two:hover .slid2{
        display: block;
    }
    li.three:hover{
        
    }

}

</style>
