<template>
    <header>
        <div class="page-logo">
            <a href="">
                <big>HKERP</big><br>
                <small>v2.0</small>
            </a> 
            <div class="menuBtn"></div>
        </div>
        <div class="search"></div>
        <div class="userInfo"></div>
    </header>
</template>
<script>

export default {
  name: 'appheader'
}
</script>
<style lang="scss" scoped>
$height:73px;
header{
    position: fixed;
    width: 100%;
    height: $height;
    background-color: #fff;
        .page-logo{
            float: left;
            background-color: #33CCCC;
            width: 200px;
            height: 100%;
            position: relative;
            a{
                width: 112px;
                cursor: pointer;
                height: 100%;
                display: block;
                color: #fff;
                text-align: center;
                text-decoration: none;  
                 font-family: 'Arial Negreta', 'Arial Normal', 'Arial';    
                &:hover,&:focus{
                    text-decoration: none;
                }
                big{
                    top: 16px;
                    left: 20px;
                    position: absolute;
                    font-weight: 700;
                    font-size: 32px;
                    display: block;
                }
                small{
                    top: 52px;
                    left: 20px;
                    position: absolute;
                    font-weight: 400;
                    font-size: 12px;
                    display: block;
                }
            }
            .menuBtn{
                cursor: pointer;
                width: 20px;
                height: 20px;
                top: 23px;
                right: 23px;
                position: absolute;
                background-image: url(../assets/sidebar-toggler.png);
                background-position: center center;
                background-repeat: no-repeat;
                opacity: 0.7;
                filter: alpha(opacity=70);
            }
            .menuBtn:hover{
                opacity: 1;
                filter: alpha(opacity=100);
            }
    }
}

</style>
