<template>

        <h2 v-if="this.$store.state.verify == 0">กรุณารอแอดมินยืนยัน</h2>
        <div v-if="this.$store.state.verify == 1" class="col-md-8 col-md-offset-2">

            <h1>โพสงาน</h1>

            <form>
                <ImagePreview v-show="this.$store.state.PostPhotoPreview" />
                <Loading v-show="loading" />
                <div class="form-group has-error">
                    <label for="Name">ชื่องาน <span class="require"></span></label>
                    <input type="text" class="form-control" name="Name" v-model="posttitle" />

                </div>
                <div class="form-group upload-file">
                    <label for="Post-photo" class="bn3">อัพโหลดรูปภาพเสริม</label>
                    <input type="file" ref="PostPhoto" id="Post-photo" @change="fileChange" accept=".png, .jpg, ,jpeg" />
                    <button @click.prevent="openPreview" class="preview bn3"
                        :class="{ 'button-inactive': !this.$store.state.PostPhotoFileURL }">
                        ดูรูปภาพ
                    </button>
                    <span>ไฟล์ที่เลือก: {{ this.$store.state.PostPhotoName }}</span>
                </div>
                <div class="form-group">
                    <label for="type">ลักษณะงาน </label>
                    <select class="form-select" aria-label="Default select example" v-model="posttype">
                        <option v-for="(type,index) in getPostDetailType" :key="index.id" :value="type" >{{ type }}</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="type">ระดับชั้นการศึกษา </label>
                    <select class="form-select" aria-label="Default select example" v-model="postEducation">
                        <option v-for="(Education,index) in getPostDetailEducation" :key="index.id" :value="Education" >{{ Education }}</option>
                    </select>
                </div>

                <div class="form-group">
  <label for="title">เงินเดือน</label>
  <input type="number" class="form-control" name="title" v-model.number="postsalary" step="1" />
</div>
<div class="form-group">
  <label for="title">จำนวนคน</label>
  <input type="number" class="form-control" name="title" v-model.number="Vacancy" step="1" />
</div>

                <div class="form-group">
                    <label for="title">รายละเอียดและสวัสดิการ</label>
                    <VueEditor :editor-toolbar="customToolbar" v-model="PostHTML" />
                </div>
                <div :class="{ invisible: !error }" class="err-message">
                    <p><span>ผิดพลาด:</span>{{ this.errorMsg }}</p>
                </div>
                <div class="form-group">
                    <button @click.prevent="uploadPost" class="btn btn-primary">สร้าง</button>
                </div>


            </form>
        </div>


 
</template>

<script>
import { VueEditor, Quill } from "vue3-editor";
import ImagePreview from "../components/ImagePreview.vue";
import { doc, setDoc, serverTimestamp, collection } from "firebase/firestore";
import { db } from '../firebase'
import { getStorage, ref, uploadBytesResumable, getDownloadURL } from "firebase/storage";
import Loading from "../components/Loading";

export default {
    created(){
        this.$store.dispatch('getPostDetail')
    },
    name: "CreateJob",
    data() {
        return {
            file: null,
            error: null,
            errorMsg: null,
            loading: null,
            downloadURL:"",
            customToolbar: [
                ["bold", "italic", "underline"],
                [{ list: "ordered" }, { list: "bullet" }],
                ["code-block"]
            ],
        };
    },
    components: {
        VueEditor,
        ImagePreview,
        Loading,

    },

    methods: {
        fileChange() {
            this.file = this.$refs.PostPhoto.files[0];
            const fileName = this.file.name;
            this.$store.commit("fileNameChange", fileName);
            this.$store.commit("createFileURL", URL.createObjectURL(this.file));
        }

        ,
        openPreview() {
            this.$store.commit("openPhotoPreview");
        },
        imageHandler(file, Editor, cursorLocation, resetUploader) {
            const stroge = getStorage()
            const storageRef = ref(stroge, `documents/PostPhotos/${file.name}`);
            const uploadTask = uploadBytesResumable(storageRef, file);

            uploadTask.on("state_changed", (snapshot) => { console.log(snapshot); },
                (err) => {
                    console.log(err);
                },
                async () => {
                    const downloadURL = getDownloadURL(uploadTask.snapshot.ref);
                    Editor.insertEmbed(cursorLocation, "image", downloadURL);
                    resetUploader();
                }
            );
        },
        uploadPost() {
  if (this.posttitle.length !== 0 && this.PostHTML.length !== 0 && this.posttype !== "" && this.postEducation !== "" && Number.isInteger(parseInt(this.postsalary)) && parseInt(this.postsalary) > 0 && Number.isInteger(parseInt(this.Vacancy)) && parseInt(this.Vacancy) > 0) {
    if (this.file) {
      this.loading = true;
      const stroge = getStorage()
      
      const storageRef = ref(stroge, `documents/PostCoverPhotos/${this.$store.state.PostPhotoName}`);
      const uploadTask = uploadBytesResumable(storageRef, this.file);

      uploadTask.on("state_changed", (snapshot) => { console.log(snapshot); },
        (err) => {
          console.log(err);
          this.loading = false;
        },
        async () => {
          getDownloadURL(uploadTask.snapshot.ref).then(async url => {
            const newDocRef = doc(collection(db, "post"))
            this.downloadURL = url;
            setDoc(
              newDocRef,
              {
                PostID: newDocRef.id,
                PostHTML: this.PostHTML,
                posttitle: this.posttitle,
                postsalary: parseInt(this.postsalary),
                posteducation: this.postEducation,
                posttype: this.posttype,
                profileId: this.ProfileId,
                PostCoverPhoto: url,
                postvacancy: parseInt(this.Vacancy),
                PostCoverPhotoName: this.PostCoverPhotoName,
                date:serverTimestamp(),
              }
            )

            // await this.$store.dispatch("getPost");
            this.loading = false;
            this.$router.push({ name: "Home"});
          })
        }
      );
      return;
    }
    this.error = true;
    this.errorMsg = "กรุณาใส่รูปภาพเสริม!";
    setTimeout(() => {
      this.error = false;
    }, 5000);
    return;
  }
  this.error = true;
  this.errorMsg = "กรุณาใส่ทุกช่อง";
  setTimeout(() => {
    this.error = false;
  }, 5000);
  return;
},


    },
    computed: {
        ProfileId() {
            return this.$store.state.ProfileId;
        },
        posttitle: {
            get() {
                return this.$store.state.posttitle;
            }, set(payload) {
                this.$store.commit("Uploadposttitle", payload);
            }
        }, posttype: {
            get() {
                return this.$store.state.posttype;
            }, set(payload) {
                this.$store.commit("Uploadposttype", payload);
            }
        }, postEducation: {
            get() {
                return this.$store.state.posteducation;
            }, set(payload) {
                this.$store.commit("UploadpostEducation", payload);
            }
        }, postsalary: {
            get() {
                return this.$store.state.postsalary;
            }, set(payload) {
                this.$store.commit("Uploadpostsalary", payload);
            }
        }, PostHTML: {
            get() {
                return this.$store.state.PostHTML;
            },
            set(payload) {
                this.$store.commit("newPostHTML", payload);
            },
        }, PostCoverPhotoName() {
            return this.$store.state.PostPhotoName;
        },
        Vacancy: {
            get() {
                return this.$store.state.postvacancy;
            },
            set(payload) {
                this.$store.commit("UploadVacancy", payload);
            },
        },
    getPostDetailType(){
      return this.$store.state.PostDetailType
    },
    getPostDetailEducation(){
      return this.$store.state.PostDetailEducation
    }
    }
}
</script>

<style lang="scss" scoped>
h1 {
    padding: 5px;
}

.form-group {
    padding: 10px;
}

.bn3 {
    display: inline-block;
    padding: 0em 1.7em;
    border: 0.1em solid rgb(0, 0, 0);
    border-radius: 2em;
    box-sizing: border-box;
    text-decoration: none;
    color: #000000;
    text-shadow: 0 0.04em 0.04em rgba(68, 68, 68, 0.253);
    text-align: center;
    transition: all 0.2s;
    cursor: pointer;
}

.bn3:hover {
    color: black;
    background-color: rgb(255, 255, 255);
}

.upload-file {
    flex: 1;
    margin-left: 16px;
    position: relative;
    display: flex;

    input {
        display: none;
    }

    .preview {
        margin-left: 16px;
        text-transform: initial;
    }

    span {
        font-size: 12px;
        margin-left: 16px;
        align-self: center;
    }
}
</style>