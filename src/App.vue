<template>
    <div class="container">
        <!-- header -->
        <div class="header d-flex justify-content-around align-items-center my-5">
            <h1 class="fw-bold">Notes</h1>
            <button type="button" class="btn btn-dark rounded-circle" data-bs-toggle="modal"
                data-bs-target="#staticBackdrop">
                <i class="add-notes fa-solid fa-plus"></i>
            </button>
        </div>

        <!-- Modal -->
        <div class="modal fade" id="staticBackdrop" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1"
            aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <!-- <h1 class="modal-title fs-5" id="staticBackdropLabel">Modal title</h1> -->
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <textarea v-model="titleNote"></textarea>
                    </div>
                    <div class="modal-footer d-flex justify-content-center">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                            Close
                        </button>
                        <button type="button" class="btn btn-primary" data-bs-dismiss="modal" @click="createNote">
                            Add note
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <!-- card note -->
        <div class="row row-cols-1 row-cols-md-3 g-4 mx-4">
            <div class="col" v-for="(data,index) in datas" :key="index">
                <div class="card h-100">
                    <div class="card-header text-end">
                        <button class="btn btn-primary" @click="onChecked(data.title)">Checked</button>
                    </div>
                    <div class="card-body"  :style="{ backgroundColor: data.bgColor}">
                        <h5 class="card-title">{{ data.title }}</h5>
                    </div>
                    <div class="card-footer">
                        <small class="text-body-secondary">{{ data.date}}</small>
                    </div>
                </div>
            </div>
        </div>
        <!-- completed notes -->
        <div class="completed-notes">
            <div class="header2 d-flex justify-content-center align-items-center my-5">
                <h1 class="fw-bold">Completed Notes</h1>
            </div>
            <div class="row px-3 pb-5 text-center d-flex align-items-center fs-2 note-done" v-for="(data, index) in dataDone" :key="index">
                <div class="col-1 p-0 pe-2">
                    <i class="fa-solid fa-check-double"></i>
                </div>
                <div class="col-11 p-0 text-start" style="word-wrap: break-word;">
                    <span class="fs-2">{{ data.title }}</span>
                </div>
            </div>
            <!-- <ul class="row fs-3 p-0 text-center">
                <li>
                    <div class="card h-100 border border-0">
                        <div class="card-body">
                                <i class="fa-solid fa-check-double "></i>
                            <p>gbrgrebgegrbgfggfegbfbretrytmy,kjhtgrfegthgjkhgfrgthgdsighosjfsohgfaosivjosf1</p>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="card h-100 border border-0">
                        <div class="card-body">
                                <i class="fa-solid fa-check-double "></i>
                            <p>gbrgrebhgdsighosjfsohgfaosivjosf1</p>
                        </div>
                    </div>
                </li>
            </ul> -->
        </div>
    </div>
</template>
<!-- chưa tối ưu code.
     tại sao truyền num[type=string] vào datas.value.splice(num, 1); vẫn thự hiện được dù có bug.
     chưa xử lý được thêm giá trị vào dataDone chỉ gồm mỗi title mà không phải lấy hết giá trị của datas.
-->
<script>
import { ref } from "vue";
export default {
    setup() {
        const datas = ref([]);
        const dataDone = ref([]);
        const titleNote = ref('');

        const createNote = () => {
            let mycolor = "#" + ((Math.random() * 0xffffff) << 0).toString(16);
            let createDate = new Date();
            let text = titleNote.value;
            datas.value.push(
                {
                    title : text,
                    date : createDate.toLocaleString(),
                    bgColor : mycolor
                })
            titleNote.value ='';
        };
        
        const onChecked = (num) => {
            let a = Number(datas.value.findIndex(x => x.title == num));
            dataDone.value.push(datas.value[a]);
            datas.value.splice(a, 1);
            console.log(dataDone.value);
        }
        return {
            createNote,
            datas,
            dataDone,
            titleNote,
            onChecked
        };
    },
};
</script>
<style>
*{
    box-sizing: border-box;
}
:root {
    --color-1: #a4c3a2;
    --color-2: #b0d4b8;
    --color-3: #eae7d6;
    --color-4: #d7f9fa;
}

::selection {
    background-color: var(--color-2);
}

.header {
    box-shadow: 0 10px 30px -16px rgba(0, 0, 0, 0.5);
}
.header2 {
    box-shadow: 0 18px 70px -8px rgba(0, 0, 0, 0.3)
}
.add-notes:hover {
    color: var(--color-2);
}

textarea {
    width: 100%;
    height: 10rem;
    outline: 2px solid var(--color-1);
    font-size: 2.2rem;
    border: none;
}
ul li{
    list-style-type: none;
    padding: .5rem 0;
}
.note-done:hover{
    transform: scale(1.04);
    transition: all .3s;
    transition-delay: .2s;
    color: #36c42d;
}
</style>