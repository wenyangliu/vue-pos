<template>
    <div>
        <!--<div class="bg">点击上传</div>-->
        <!--<input type="file" class="input-file" name="fulAvatar" @change="changeImage($event)" accept="image/gif,image/jpeg,image/jpg,image/png">-->
        <!--<img :src="avatar" alt="" name="avatar">-->
        <!--<div class="text" @click="upload" v-if="file">确定上传</div>-->

        <div class="image-view">
            <div class="addbox">
                <input type="file" ref="files" @change="changeImage($event)">
                <div class="addbtn">+</div>
            </div>
            <div class="view">
                <div class="item" v-for="(item, index) in imgBase64">
                    <span class="cancel-btn" @click="delImg(index)">x</span>
                    <img :src="item">
                </div>
            </div>
        </div>
        <button class="text" @click="upload">确定上传</button>
    </div>

</template>

<script>
    import axios from 'axios'

    let formData
    export default {
        name: 'Pos',
        data() {
            return {
                avatar: '',
                file: '',
                imgBase64: []
            }
        },
        created() {
            this.avatar = this.imgUrl
        },
        methods: {
            changeImage(e) {
                console.log(e)
                const _this = this
                const file = e.target.files[0]
                formData = new FormData()
                formData.append('file', file)
                console.log(_this.filesArr)
                const reader = new FileReader()
                reader.onload = function (e) {
                    _this.imgBase64.push(e.target.result)
                }
                reader.readAsDataURL(file)
            },
            delImg(index) {
                this.imgBase64.splice(index, 1)
            },
            upload() {
                axios.post('http://localhost:3000/api/upload', formData)
                    .then(res => {
                        console.log(res)
                    })
                    .catch(err => {
                        console.log(err)
                    })
            }

            // changeImage(e) {
            //   console.log(e)
            //   const file = e.target.files[0]
            //   const formData = new FormData()
            //   formData.append('file', file)
            //   axios.post('http://localhost:3000/api/upload', formData)
            //     .then(res => {
            //       console.log(res)
            //     })
            //     .catch(err => {
            //       console.log(err)
            //     })
            // }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    * {
        margin: 0 auto;
        padding: 0;
        font-family: "微软雅黑";
    }

    .clearboth::after {
        content: "";
        display: block;
        clear: both;
    }

    .image-view {
        width: 400px;
        height: 300px;
        margin: 50px auto;
    }

    .image-view .addbox {
        float: left;
        position: relative;
        height: 100px;
        width: 100px;
        margin-bottom: 20px;
        text-align: center;
    }

    .image-view .addbox input {
        position: absolute;
        left: 0;
        height: 100px;
        width: 100px;
        opacity: 0;
    }

    .image-view .addbox .addbtn {
        float: left;
        height: 100px;
        width: 100px;
        line-height: 100px;
        color: #fff;
        font-size: 40px;
        background: #ccc;
        border-radius: 10px;
    }

    .image-view .item {
        position: relative;
        float: left;
        height: 100px;
        width: 100px;
        margin: 10px 10px 0 0;
    }

    .image-view .item .cancel-btn {
        position: absolute;
        right: 0;
        top: 0;
        display: block;
        width: 20px;
        height: 20px;
        color: #fff;
        line-height: 20px;
        text-align: center;
        background: red;
        border-radius: 10px;
        cursor: pointer;
    }

    .image-view .item img {
        width: 100%;
        height: 100%;
    }

    .image-view .view {
        clear: both;
    }
</style>
