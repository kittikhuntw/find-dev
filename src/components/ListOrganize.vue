<template>
    <div v-if="post.verify == 0" class="container-xl py-1">
                        <div class="job-item p-4 mb-4">
                            <div class="row g-4">
                                <div class="col-md-1 col-12 d-flex align-items-center">
                                    <img class="flex-shrink-0 img-fluid border rounded" :src="post.profileimage" alt=""
                                        style="width: 80px; height: 80px;">
                                    <div class="text-start ps-4">
                                        <h5 class="mb-3">บริษัท {{ post.name }}</h5>
                                    </div>
                                </div>
                                <div
                                    class="col-md-11 col-12 d-flex flex-column align-items-end justify-content-center">
                                    <div class="d-flex mb-3">
                                        <router-link
                                            :to="{ name: 'DetailOrganize', params: { OrganizeID: this.post.profileID } }"
                                            class="button-3">ดูรายละเอียด</router-link>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
</template>
  
<script>
export default {
    name: "Listorganize",
    props: ["post"],
}
</script>
  
<style lang="scss" scoped>
.nav-pills .nav-item .active {
    border-bottom: 2px solid var(--primary);
}

.job-item {
    border: 1px solid transparent;
    border-radius: 2px;
    box-shadow: 0 0 45px rgba(0, 0, 0, .08);
    transition: .5s;
}

// .job-item:hover {
//   border-color: rgba(0, 0, 0, .08);
//   box-shadow: none;
// }

.button-3 {
    appearance: none;
    background-color: #2ea44f;
    border: 1px solid rgba(27, 31, 35, .15);
    border-radius: 6px;
    box-shadow: rgba(27, 31, 35, .1) 0 1px 0;
    box-sizing: border-box;
    color: #fff;
    cursor: pointer;
    display: inline-block;
    font-family: -apple-system, system-ui, "Segoe UI", Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji";
    font-size: 14px;
    font-weight: 600;
    line-height: 20px;
    padding: 6px 16px;
    position: relative;
    text-align: center;
    text-decoration: none;
    user-select: none;
    -webkit-user-select: none;
    touch-action: manipulation;
    vertical-align: middle;
    white-space: nowrap;
}

.button-3:focus:not(:focus-visible):not(.focus-visible) {
    box-shadow: none;
    outline: none;
}

.button-3:hover {
    background-color: #2c974b;
}

.button-3:focus {
    box-shadow: rgba(46, 164, 79, .4) 0 0 0 3px;
    outline: none;
}

.button-3:disabled {
    background-color: #94d3a2;
    border-color: rgba(27, 31, 35, .1);
    color: rgba(255, 255, 255, .8);
    cursor: default;
}

.button-3:active {
    background-color: #298e46;
    box-shadow: rgba(20, 70, 32, .2) 0 1px 0 inset;
}

.button-2 {
    appearance: none;
    background-color: #aa2200;
    border: 1px solid rgba(27, 31, 35, .15);
    border-radius: 6px;
    box-shadow: rgba(27, 31, 35, .1) 0 1px 0;
    box-sizing: border-box;
    color: #fff;
    cursor: pointer;
    display: inline-block;
    font-family: -apple-system, system-ui, "Segoe UI", Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji";
    font-size: 14px;
    font-weight: 600;
    line-height: 20px;
    padding: 6px 16px;
    position: relative;
    text-align: center;
    text-decoration: none;
    user-select: none;
    -webkit-user-select: none;
    touch-action: manipulation;
    vertical-align: middle;
    white-space: nowrap;
}</style>