<template>
    <footer>
        <div class="container">
            <div class="class left">
                <div class="class col-1">
                    <router-link class="header" to="">Find Dev</router-link>
                    <ul>
                        <li>
                            <router-link class="link" :to="{ name: 'Help' }">ติดต่อผู้พัฒนา</router-link>
                        </li>
                    </ul>
                </div>
            </div>
        </div> 
    </footer>
</template>

<script>
export default {

}
</script>

<style lang="scss" scoped>
footer{
    margin-top:auto;
    padding: 100px 25px;
    background-color: #000000;
}

</style>